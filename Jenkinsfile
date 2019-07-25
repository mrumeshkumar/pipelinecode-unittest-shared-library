pipeline { 
    agent {
        docker { image 'zenika/alpine-maven' }
    } 
 //   tools { 
   //     maven 'Maven 3.6.0' 
    //    jdk 'jdk8' 
   // }
    options {
        skipStagesAfterUnstable()
    }
    stages { 
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }
        stage ('SonarScan'){
             steps {
                sh' mvn sonar:sonar \
                        -Dsonar.projectKey=mrumeshkumar_pipelinecode-unittest-shared-library \
                        -Dsonar.organization=mrumeshkumar-github \
                        -Dsonar.host.url=https://sonarcloud.io \
                        -Dsonar.login=bed657b793f86eec907ea8d6b9014e66d35f53f3'
             }
        }
        
        stage('Build') { 
            steps { 
               echo 'This is a minimal pipeline.' 
               // sh 'mvn -Dmaven.test.failure.ignore=true install'
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                 echo 'mvn test' 
                 sh 'mvn test'
            }
            post {
                always {
                      echo 'mvn test Post' 
                      junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                  echo 'mvn Deliver build output' 
              //  sh './jenkins/scripts/deliver.sh'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/*.jar', fingerprint: true
                    junit 'target/surefire-reports/*.xml'
                       }
            }
        }
        stage('Deploy - Staging') {
            steps {
                   // sh './deploy staging'
                  //  sh './run-smoke-tests'
                    echo './deploy staging'
                    echo './run-smoke-tests'
                }
        }
        stage('Sanity check') {
            steps {
                input "Does the staging environment look ok?"
            }
        }
        stage('Deploy - Production') {
            steps {
                echo  './deploy production'
               // sh './deploy production'
            }
        }
    }
    post {
    failure {
    //    mail to: 'mrumeshkumar@hotmail.com',
      //       subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
      //       body: "Something is wrong with ${env.BUILD_URL}"

       echo  "Failed Pipeline: ${currentBuild.fullDisplayName}"
    }
}
}