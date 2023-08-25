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
             docker build -t oumarkenneh/DB .
             cd $WORKSPACE
             cd UI
             docker build -t oumarkenneh/UI .
             cd $WORKSPACE
             cd auth
             docker build -t oumarkenneh/auth .
             cd $WORKSPACE
             cd weather
             docker build -t oumarkenneh/weather .
             cd $WORKSPACE
             '''
            }
         }
    }
}

s