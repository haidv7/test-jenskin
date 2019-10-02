pipeline {
  agent any

  stages {
    stage('Confirm Relealse') {
      steps {
        script {
            confirm_release = input message: 'Do you want to release a new version',
                                    ok: 'Release',
                                    parameters: [
                                        [$class: 'TextParameterDefinition',
                                        defaultValue: '',
                                        description: 'Version to be released',
                                        name: 'version'
                                        ]
                                    ]
            echo "Answer: $confirm_release"
        }
      }
    }
  }
}