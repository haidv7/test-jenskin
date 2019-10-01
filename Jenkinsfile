pipeline {
    agent any

    triggers {
        GenericTrigger(
            genericVariables: [
             [expressionType: 'JSONPath', key: 'body', value: '$'],
             [expressionType: 'JSONPath', key: 'reference', value: '$.ref'],
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
                echo "Action: $body"
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
