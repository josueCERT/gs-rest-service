pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        bat 'gradlew build -x test'
      }
    }
    stage('test') {
      steps {
        bat 'gradlew test '
      }
      post { //post build action for the stage
            always {
             junit '/build/test-results/test/*.xml'
            }
       }
    }
    stage('build image') {
          steps {
            bat 'gradlew docker '
          }
     }
    stage('scan image') {
         parallel {
            /*
            stage('scan with AquaMicroscanner') {
                                steps {
                                  // aquaMicroscanner imageName: 'com.example/rest-service', notCompliesCmd: '', onDisallowed: 'fail', outputFormat: 'html'
                                   catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                                                      aquaMicroscanner imageName: 'com.example/rest-service', notCompliesCmd: 'exit 1', onDisallowed: 'fail', outputFormat: 'html'
                                   }
                                }
             }
             */


              stage('scan with Clair') {
                                             steps {
                                               // bat 'docker run  -d --name db arminc/clair-db'
                                               // bat 'timeout /t 15'
                                               // bat ' docker run -p 6060:6060 --link db:postgres -d --name clair arminc/clair-local-scan'
                                               // bat 'timeout /t 1'
                                               // bat 'wget -qO clair-scanner.exe https://github.com/arminc/clair-scanner/releases/download/v12/clair-scanner_windows_386.exe'
                                               bat 'curl -L https://github.com/arminc/clair-scanner/releases/download/v12/clair-scanner_windows_386.exe > clair-scanner.exe'
                                               // bat 'echo Y | cacls clair-scanner.exe /g everyone:f'
                                               bat 'clair-scanner --ip=172.18.41.161 com.example/rest-service'
                                             }
                          }
         }
    }
      stage('publish image') {
                          steps {
                            bat 'echo "publish image"'
                          }
      }
       stage('deploy to test') {
                                steps {
                                  bat 'echo "deploy to test"'
                                }
       }
  }
}