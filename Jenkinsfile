pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh 'whoami'
        sh './gradlew build -x test'
      }
    }
    stage('test') {
      steps {
        sh './gradlew test '
      }
      post { //post build action for the stage
            always {
             junit 'build/test-results/test/*.xml'
            }
       }
    }
    stage('build image') {
          steps {
            sh './gradlew docker '
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

}*/

              stage('scan with Clair') {


                                             steps {
                                                  catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                                                        sh '''
                                                              docker run -d --name db arminc/clair-db || true
                                                              sleep 15 # wait for db to come up
                                                              docker run -p 6060:6060 --link db:postgres -d --name clair arminc/clair-local-scan || true
                                                              sleep 1
                                                              DOCKER_GATEWAY=$(docker network inspect bridge --format "{{range .IPAM.Config}}{{.Gateway}}{{end}}")
                                                              wget -qO clair-scanner https://github.com/arminc/clair-scanner/releases/download/v12/clair-scanner_linux_amd64 && chmod +x clair-scanner
                                                              ./clair-scanner --ip="$DOCKER_GATEWAY" com.example/rest-service
                                                            '''
                                                    }
                                             }
               }

         }
    }
      stage('publish image') {
                          steps {
                            sh 'echo "publish image"'
                          }
      }
       stage('deploy to test') {
                                steps {
                                  sh 'echo "deploy to test"'
                                }
       }
  }
}