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

     stage('scan image (aquaMicroscanner)') {
                    steps {
                      // aquaMicroscanner imageName: 'com.example/rest-service', notCompliesCmd: '', onDisallowed: 'fail', outputFormat: 'html'
                      aquaMicroscanner imageName: 'com.example/rest-service', notCompliesCmd: 'exit 0', onDisallowed: 'fail', outputFormat: 'html'
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