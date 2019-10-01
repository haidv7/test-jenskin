pipeline {
  agent any
  triggers {
    GenericTrigger(
     genericVariables: [
      [key: 'ref', value: '$.ref'],
      [key: 'repository', regexpFilter: '[^a-z_-]', value: '$.repository']
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
          echo repository ${repository}
        """
      }
    }
  }
}