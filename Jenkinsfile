pipeline {
    agent any

    triggers {
        GenericTrigger(
            genericVariables: [
            [key: 'payload', value: '$'],
            ],
            causeString: 'Triggered',
            regexpFilterExpression: '',
            regexpFilterText: '',
            printContributedVariables: true,
            printPostContent: true
        )
  }

    stages {
        stage('Test') {
            steps {
                echo "Action: ${payload}"
                echo 'Running tests ...'
            }
        }

        stage('Deploy') {
            steps {
                //TODO: validate semver version
                // if ("$IMAGE_VERSION" =~ '[0-9]+\\.[0-9]+\\.[0-9]+') {
                //     echo 'Version does not meet specification'
                //     proccess.exit(1)
                // }
                echo 'Building and pushing image ...'
              
            }
        }

        stage('Tagging') {
            steps {
                echo 'Tagging ...'

                // TODO: updating package.json version
            }
        }
    }
}
