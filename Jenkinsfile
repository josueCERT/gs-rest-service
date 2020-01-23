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
         junit '/test-results/test/*.xml'
        }

       }
    }

  }
}