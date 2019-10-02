pipeline {
  agent any

  stages {
    stage('Confirm Relealse') {
      steps {
        script {
            release_version = input message: 'Do you want to release a new version',
                                    ok: 'Release',
                                    parameters: [
                                        [$class: 'TextParameterDefinition',
                                        defaultValue: '',
                                        description: 'Version to be released',
                                        name: 'version'
                                        ]
                                    ]
            if (release_version == '') {
                currentBuild.result = 'SUCCESS'
            }
            echo "Answer: $release_version"
        }
      }
    }
  }
}