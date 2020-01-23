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

     stage('Aqua image scanner') {
                    steps {
                      // aquaMicroscanner imageName: 'com.example/rest-service', notCompliesCmd: '', onDisallowed: 'fail', outputFormat: 'html'
                      aquaMicroscanner imageName: 'com.example/rest-service', notCompliesCmd: 'exit 1', onDisallowed: 'ignore', outputFormat: 'html'
                    }
      }


  }
}