pipeline {
    agent any

    triggers {
        GenericTrigger(
            genericVariables: [
            [key: 'action', value: '$.action'],
            [key: 'merged', value: '$.merged']
            [key: 'sourceBranch', value: '$.head.ref'],
            [key: 'targetBranch', value: '$.base.ref']
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
                echo "Action: ${action}"
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
