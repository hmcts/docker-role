#!groovy

properties(
  [[$class: 'GithubProjectProperty', projectUrlStr: 'https://github.com/hmcts/docker-role/'],
   pipelineTriggers([[$class: 'GitHubPushTrigger']])]
)

@Library('Reform') _

node {
  ws('docker-role') { // This must be the name of the role otherwise ansible won't find the role
    try {
      wrap([$class: 'AnsiColorBuildWrapper', colorMapName: 'xterm']) {
        stage('Checkout') {
          checkout scm
        }

        stage('Kill any existing jobs') {
          sh '''
          molecule destroy
        '''
        }

        stage('Start containers') {
          sh '''
          molecule create
        '''
        }

        stage('Run tests') {
          sh '''
          molecule syntax
          molecule verify
        '''
        }

        stage('Run role') {
          sh '''
          molecule converge
        '''
        }

        stage('Check idempotent') {
          sh '''
          molecule idempotence
        '''
        }

      }

    } catch (err) {
      notifyBuildFailure channel: '#devops'
      throw err
    } finally {
      stage('Cleanup') {
        sh '''
         molecule destroy
        '''
      }
      deleteDir()
    }
  }
}
