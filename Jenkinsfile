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
     /*   stage ('SonarScan'){
             steps {
                bat ' mvn sonar:sonar \
                        -Dsonar.projectKey=mrumeshkumar_pipelinecode-unittest-shared-library \
                        -Dsonar.organization=mrumeshkumar-github \
                        -Dsonar.host.url=https://sonarcloud.io \
                        -Dsonar.login=33d4ddfc5d686070b7a872d5a737a20affb0060c'
             }
        }*/
        
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