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
                currentBuild.result = 'SUCCESS'
            }

            // TODO: validate version
            // isVerionValid = (release_version ==~ /Patch_For_(\d+\.)?(\d+\.)?(\d+)/)
            // if (isVerionValid) {
            //     currentBuild.result = 'FAILURE'
            // }

          
            echo "====== Preparing to release new version: $release_version ======"

            // Create pull request from lastest commit in the current branch
            echo "Creating new pull request from ${env.BRANCH_NAME} to master ..."
            
            withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId:'hai.dinh', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
              repo = scm.getUserRemoteConfigs()[0].getUrl().tokenize('/')[3].split("\\.")[0]
              repo_owner = scm.getUserRemoteConfigs()[0].getUrl().tokenize('/')[2]

              sh """
                curl -v -u "$USERNAME:$PASSWORD" -H 'Content-Type: application/json' -X POST "https://api.github.com/repos/${repo_owner}/${repo}/pulls" -d '{"title":"Release ${release_version}","body": "Release ${release_version}","head": "${env.BRANCH_NAME}","base": "master"}'
              """
            }
            echo "Done."

            // Merge pull request and tag verion
            echo "Merging pull request from ${env.BRANCH_NAME} to master ..."
            sh "git checkout master"
            sh "git merge --no-ff ${env.BRANCH_NAME}"
            sh "git tag ${release_version}"
            sh "git push origin master"
            sh "git push origin ${release_version}"
            echo "Done"
        }
      }
    }
  }
}