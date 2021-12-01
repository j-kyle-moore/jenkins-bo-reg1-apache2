pipeline {
  agent {
    node {
      label 'kaniko-yaml'
    }

  }
  stages {
    stage('checkout SCM') {
      steps {
        git(url: 'https://github.com/j-kyle-moore/jenkins-bo-reg1-apache2.git', branch: 'master', credentialsId: 'jkm-github')
      }
    }

  }
  environment {
    SCM_SOURCE = 'Bitbucket'
    SCM_REPO_NAME = 'jenkins-pull-kaniko'
    SCM_REPO_CREDS = 'j7-bitbucket'
  }
}