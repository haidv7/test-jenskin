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
            message = sh(script: 'git log -1 --pretty=%B', returnStdout: true).trim()
            sh """
                echo reference ${ref}
                echo message ${message} 
            """
        }
      }
    }
  }
}