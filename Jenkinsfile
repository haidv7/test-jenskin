pipeline {
  agent any
  triggers {
    GenericTrigger(
     genericVariables: [
      [expressionType: 'JSONPath', key: 'ref', value: '$.ref'],
      [expressionType: 'JSONPath', key: 'message', value: '$.head_commit.message'],
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
        sh """
          echo reference ${ref}
          echo message ${message}
        """
      }
    }
  }
}