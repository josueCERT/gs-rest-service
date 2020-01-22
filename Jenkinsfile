pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        withGradle() {
          bat './complete/gradlew test'
        }

      }
    }

  }
}