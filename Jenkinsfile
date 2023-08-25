pipeline {
    agent {
        label 'node'
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('Dokerhub')
    }
    stages {
      stage('clone') {
         steps {
             sh '''
             ls 
             pwd
             '''
         }  
     }
         stage('SonarQube analysis') {
            agent {
                docker {
                  image 'sonarsource/sonar-scanner-cli:4.7.0'
                }
               }
               environment {
        CI = 'true'
        //  scannerHome = tool 'Sonar'
        scannerHome='/opt/sonar-scanner'
    }
            steps{
                withSonarQubeEnv('Sonar') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }

      stage('buildAndTagImages ') {
         steps {
             sh '''
             cd DB
             docker build -t oumarkenneh/db1 .
             cd $WORKSPACE
             cd UI
             docker build -t oumarkenneh/ui1 .
             cd $WORKSPACE
             cd auth
             docker build -t oumarkenneh/auth1 .
             cd $WORKSPACE
             cd weather
             docker build -t oumarkenneh/weather1 .
             '''
            }
         }
        stage('Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'Dokerhub',
                        usernameVariable: 'DOCKER_HUB_USERNAME',
                        passwordVariable: 'DOCKER_HUB_PASSWORD'
                    )
                ]) {
                    sh "docker login -u ${DOCKER_HUB_USERNAME} -p ${DOCKER_HUB_PASSWORD}"
                }
            }
        }
      stage('publish to dockerhub') {
         steps {
             sh '''
             docker push oumarkenneh/db1 
             docker push oumarkenneh/ui1 
             docker push oumarkenneh/auth1 
             docker push oumarkenneh/weather1 
             '''
            }
         }
    }
}




