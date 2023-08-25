pipeline {
    agent {
        label 'node'
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
             docker build -t oumarkenneh/db .
             cd $WORKSPACE
             cd UI
             docker build -t oumarkenneh/ui .
             cd $WORKSPACE
             cd auth
             docker build -t oumarkenneh/auth .
             cd $WORKSPACE
             cd weather
             docker build -t oumarkenneh/weather .
             '''
            }
         }
      stage('publish to dockerhub') {
         steps {
             sh '''
             docker push oumarkenneh/db 
             docker push oumarkenneh/ui 
             docker push oumarkenneh/auth 
             docker push oumarkenneh/weather 
             '''
            }
         }
    }
}

