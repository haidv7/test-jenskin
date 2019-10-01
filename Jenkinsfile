pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                echo 'Running tests ...'
            }
        }

        stage('Deploy') {
            when {
               beforeAgent true
               branch 'master'
            }

            input {
                id: 'image_version'
                message 'Input version number to be realeased'
                ok: 'Release' 
                parameters {
                    string(name: 'IMAGE_VERSION', description: 'Version number to be released, which must be following the semantic version(https://semver.org/)')
                }
            }

            steps {
                //TODO: validate semver version
                // if ("$IMAGE_VERSION" =~ '[0-9]+\\.[0-9]+\\.[0-9]+') {
                //     echo 'Version does not meet specification'
                //     proccess.exit(1)
                // }
                echo 'Building and pushing image ...'
                echo "IMAGE_VERSION=${IMAGE_VERSION}"
              
            }
        }

        stage('Tagging') {
            when {
               branch 'master'
            }

            steps {
                echo 'Tagging ...'

                // TODO: updating package.json version
            }
        }
    }
}
