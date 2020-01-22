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
    }

  }
}