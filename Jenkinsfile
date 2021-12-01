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

    stage('Build and push to Harbor with Kaniko') {
      steps {
        sh '''
        /kaniko/executor --dockerfile `pwd`/Dockerfile  --context `pwd` --destination=${HARBOR_SERVER}/${HARBOR_REPO1}/${IMAGE_NAME}:${IMAGE_TAG} --destination=${HARBOR_SERVER}/${HARBOR_REPO1}/${IMAGE_NAME}:$BUILD_NUMBER --verbosity=${LOG_LEVEL} --registry-certificate ${HARBOR_SERVER}=${REGISTRY_CERT_LOC}
        '''
      }
    }

  }
  environment {
    SCM_SOURCE = 'Bitbucket'
    SCM_REPO_NAME = 'jenkins-pull-kaniko'
    SCM_REPO_CREDS = 'j7-bitbucket'
    SCM_REPO_URL = 'bitbucket.di2e.net/scm/ddjtdev/jenkins-pull-kaniko.git'
    SCM_REPO_BRANCH = 'DDJTDEV-2083-jenkins-pipeline-for-pulling-kaniko'
    JENKINS_SERVER = 'jenkins-commercial.rke2-app.km.test'
    JENKINS_PIPELINE_NAME = 'jenkins-pull-kaniko'
    HARBOR_SERVER = 'harbor.rke2-app.km.test'
    HARBOR_REPO1 = 'ead_base_images'
    HARBOR_REPO2 = 'eaddev'
    IMAGE_NAME = 'kaniko_debug'
    IMAGE_TAG = 'latest'
    REGISTRY_CERT_LOC = '/kaniko/ssl/km-test-certs/km_test_ca.crt'
    LOG_LEVEL = 'debug'
    CLAMAV_FILES = '/home/jenkins/agent/workspace/*'
    EMAIL_RECPTS = 'moore.kyle@idsi.com'
  }
}
