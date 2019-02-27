pipeline {
  options {
    disableConcurrentBuilds()
  }
  agent {
    label "jenkins-maven"
  }
  environment {
    DEPLOY_NAMESPACE = "staging"
    GIT_MESSAGE = ""
  }
  stages {
    stage('Validate Environment') {
      steps {
        container('maven') {
          dir('env') {
            env.GIT_MESSAGE = sh (script:'git log --oneline -1 ${GIT_COMMIT}', returnStatus: true)
            sh 'jx step helm build'
          }
        }
      }
    }
    stage('Update Environment') {
      when {
        branch 'master'
      }
      steps {
        container('maven') {
          dir('env') {
            sh 'jx step helm apply'
          }
        }
      }
    }
  }
  post {
    success {
          rocketSend attachments: [
            [
              audioUrl: '',
              authorIcon: '',
              authorName: '',
              color: 'green',
              imageUrl: '',
              messageLink: '',
              text: 'Success',
              thumbUrl: '',
              title: "Commit: ${env.GIT_MESSAGE}",
              titleLink: '',
              titleLinkDownload: '',
              videoUrl: ''
            ]
          ],
          channel: 'staging'
          message: "Deploy into Staging #${env.BUILD_NUMBER} finished - (<${env.BUILD_URL}|Open>)",
          rawMessage: true
        }
        failure {
          rocketSend attachments: [
            [
              audioUrl: '',
              authorIcon: '',
              authorName: '',
              color: 'red',
              imageUrl: '',
              messageLink: '',
              text: 'Failure',
              thumbUrl: '',
              title: "Commit: ${env.GIT_MESSAGE}",
              titleLink: '',
              titleLinkDownload: '',
              videoUrl: ''
            ]
          ],
          channel: 'staging'
          message: "Deploy into Staging #${env.BUILD_NUMBER} finished - (<${env.BUILD_URL}|Open>)",
          rawMessage: true
        }
  }
}
