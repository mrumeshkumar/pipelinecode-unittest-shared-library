pipeline { 
 //   agent {
 //       docker { image 'zenika/alpine-maven' }
  //  } 
   agent any

    tools { 
        maven 'Maven 3.6.0' 
        jdk 'jdk8' 
    }
    options {
        skipStagesAfterUnstable()
    }
    stages { 
        stage ('Initialize') {
            steps {
                bat '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }
        stage ('SonarScan'){
             steps {
                bat ' mvn sonar:sonar \
                        -Dsonar.projectKey=mrumebatkumar_pipelinecode-unittest-batared-library \
                        -Dsonar.organization=mrumebatkumar-github \
                        -Dsonar.host.url=https://sonarcloud.io \
                        -Dsonar.login=bed657b793f86eec907ea8d6b9014e66d35f53f3'
             }
        }
        
        stage('Build') { 
            steps { 
               echo 'This is a minimal pipeline.' 
               // bat 'mvn -Dmaven.test.failure.ignore=true install'
                bat 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                 echo 'mvn test' 
                 bat 'mvn test'
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
              //  bat './jenkins/scripts/deliver.bat'
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
                   // bat './deploy staging'
                  //  bat './run-smoke-tests'
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
               // bat './deploy production'
            }
        }
    }
    post {
    failure {
    //    mail to: 'mrumebatkumar@hotmail.com',
      //       subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
      //       body: "Something is wrong with ${env.BUILD_URL}"

       echo  "Failed Pipeline: ${currentBuild.fullDisplayName}"
    }
}
}