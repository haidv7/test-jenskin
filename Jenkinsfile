pipeline {
  agent any

  stages {
    stage('Confirm Relealse') {
      steps {
        script {
            confirm_release = input message: 'Do you want to release a new version', ok: 'Release'
            echo "Answer: $confirm_release"
        }
      }
    }
  }
}