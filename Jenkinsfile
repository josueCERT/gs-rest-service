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
            stage('scan with AquaMicroscanner') {
                                steps {
                                  // aquaMicroscanner imageName: 'com.example/rest-service', notCompliesCmd: '', onDisallowed: 'fail', outputFormat: 'html'
                                   catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                                                      aquaMicroscanner imageName: 'com.example/rest-service', notCompliesCmd: 'exit 1', onDisallowed: 'fail', outputFormat: 'html'
                                   }
                                }
             }
              stage('scan with Clair') {
                                             steps {
                                               echo '"scan with Clair"'
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