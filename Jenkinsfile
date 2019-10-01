pipeline {
  agent any
  triggers {
    GenericTrigger(
     genericVariables: [
      [expressionType: 'JSONPath', key: 'ref', value: '$.ref'],
     ],
     causeString: 'Triggered on $ref',
     regexpFilterExpression: 'generic $ref',
     regexpFilterText: '',
     printContributedVariables: true,
     printPostContent: true
    )
  }
  stages {
    stage('Test Generic Trigger') {
      steps {
        script {
            message = sh(script: 'git show -s $GIT_COMMIT --format="format:%s"', returnStdout: true).trim()
        }
        echo "reference ${ref}"
        echo "message ${message}" 
      }
    }
  }
}