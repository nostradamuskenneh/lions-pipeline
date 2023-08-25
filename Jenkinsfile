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
      stage('build Images') {
         steps {
             sh '''
             cd DB
             docker build -t DB .
             cd $WORKSPACE
             cd UI
             docker build -t UI .
             cd $WORKSPACE
             cd auth
             docker build -t auth .
             cd $WORKSPACE
             cd weather
             docker build -t weather .
             cd $WORKSPACE
             '''
            }
         }
    }
}

s