pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        withGradle() {
          bat 'cd  complete'
        }

        withGradle() {
          bat 'gradlew test'
        }

      }
    }

  }
}