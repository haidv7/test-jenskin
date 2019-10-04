pipeline {
  agent any

  stages {
    stage('Confirm Release') {
      when {
           beforeInput true
           branch "release/*"
      }

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
                currentBuild.result = 'FAILURE'
            }

            // TODO: validate version
            // isVerionValid = (release_version ==~ /Patch_For_(\d+\.)?(\d+\.)?(\d+)/)
            // if (isVerionValid) {
            //     currentBuild.result = 'FAILURE'
            // }
        }
      }
    }

    stage('Bumping version') {
      when {
         branch "release/*"
         expression { release_version != '' }
      }

      steps {
        // Bumping version in package.json
        echo "Bumping version in package.json to ${release_version}"

        script {
          sh """
            sed -i 's/"version": .*,/"version": "${release_version}",/' package.json
            sed -i 's/export IMAGE_VERSION=.*/export IMAGE_VERSION=${release_version}/' docker/.bin/.env.sh
            git add package.json
            git add docker/.bin/.env.sh
            git commit -m "Bumping version to ${release_version}"
          """
        }

        echo "Done."
      }
    }

    stage('Creating PR') {
      when {
         branch "release/*"
         expression { release_version != '' }
      }

      steps {
        // Create pull request from lastest commit in the current branch
        echo "Creating new pull request from ${env.BRANCH_NAME} to master ..."

        script {
          withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId:'hai.dinh', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
            repo = scm.getUserRemoteConfigs()[0].getUrl().tokenize('/')[3].split("\\.")[0]
            repo_owner = scm.getUserRemoteConfigs()[0].getUrl().tokenize('/')[2]

            pull_request_number = sh (
              script: """curl -s -u "$USERNAME:$PASSWORD" -H 'Content-Type: application/json' -X POST "https://api.github.com/repos/${repo_owner}/${repo}/pulls" -d '{"title":"Release ${release_version}", "body": "Release ${release_version}", "head": "${env.BRANCH_NAME}", "base": "master"}' 2>&1 | grep '"number":' | sed 's/[^0-9]*//g' """,
              returnStdout: true
            ).trim()
          }

          if (pull_request_number == '') {
            currentBuild.result = 'FAILURE'
          }
        }

        echo "Creating pull request #${pull_request_number} successfully."
        echo "Done."
      }
    }

    stage('Revert if creating PR fail') {
      when {
         expression { pull_request_number == '' }
      }

      steps {
        echo "Revert bump version commit in ${env.BRANCH_NAME}"

        sh """
          git reset --hard HEAD~1
          git push origin ${env.BRANCH_NAME} -f
        """
      }
    }

    stage('Merging PR') {
      when {
         expression { pull_request_number != '' }
      }

      steps {
        // Merge pull request
        echo "Merging pull request #${pull_request_number} to master ..."

        script {
          
          withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId:'hai.dinh', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
            repo = scm.getUserRemoteConfigs()[0].getUrl().tokenize('/')[3].split("\\.")[0]
            repo_owner = scm.getUserRemoteConfigs()[0].getUrl().tokenize('/')[2]

            merged_response_status = sh (
              script: """curl -s -LI -u "$USERNAME:$PASSWORD" -H 'Content-Type: application/json' -X PUT "https://api.github.com/repos/${repo_owner}/${repo}/pulls/${pull_request_number}/merge" -o /dev/null -w '%{http_code}\n' """,
              returnStdout: true
            ).trim()
          }

          if (merged_response_status != "200") {
              currentBuild.result = 'FAILURE'
          }
        }

        echo "Merged response status: ${merged_response_status}"
        echo "Merging pull request #${pull_request_number} successfully."
        echo "Done."
      }
    }

    stage('Tagging') {
      when {
         expression { merged_response_status == '200' }
      }

      steps {
        echo "Tagging..."

        git branch: "master", credentialsId: 'hai.dinh', url: scm.getUserRemoteConfigs()[0].getUrl()

        script {
          withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId:'hai.dinh', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
            origin_url = scm.getUserRemoteConfigs()[0].getUrl().split('//')[1]

            sh """
              git tag ${release_version}
              git remote set-url origin https://$USERNAME:$PASSWORD@${origin_url}
              git push origin ${release_version}
            """
          }
        }
        
        echo "Done."
      }
    }
  }
}