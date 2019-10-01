pipeline {
  agent any
  triggers {
    GenericTrigger(
     genericVariables: [
      [key: 'ref', value: '$.action'],
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
          echo reference ${action}
        """
      }
    }
  }
}