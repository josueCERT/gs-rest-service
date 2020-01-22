pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        withGradle() {
          bat 'gradlew test'
        }

      }
    }

  }
}