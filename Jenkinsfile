pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
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
