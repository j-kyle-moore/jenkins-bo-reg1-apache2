pipeline {
  agent {
    node {
      label 'kaniko-yaml'
    }
    environment {
      SCM_SOURCE = 'Github'
      SCM_REPO_NAME = 'jenkins-bo-reg1-apache2'
      SCM_REPO_CREDS = 'jkm-github'
      SCM_REPO_URL = 'github.com/j-kyle-moore/jenkins-bo-reg1-apache2.git'
      SCM_REPO_BRANCH = 'master'
      JENKINS_SERVER = 'jenkins-commercial.rke2-app.km.test'
      JENKINS_PIPELINE_NAME = 'jenkins-bo-reg1-apache2'
      HARBOR_SERVER = 'harbor.rke2-app.km.test'
      HARBOR_REPO1 = 'ead_base_images'
      HARBOR_REPO2 = 'eaddev'
      IMAGE_NAME = 'apache2'
      IMAGE_TAG = 'latest'
      REGISTRY_CERT_LOC = '/kaniko/ssl/km-test-certs/km_test_ca.crt'
      LOG_LEVEL = 'debug'
      CLAMAV_FILES = '/home/jenkins/agent/workspace/*'
      EMAIL_RECPTS = 'moore.kyle@idsi.com'
    }
    
  }
  stages {
    stage('checkout SCM') {
      steps {
        // git(url: 'https://github.com/j-kyle-moore/jenkins-bo-reg1-apache2.git', branch: 'master', credentialsId: 'jkm-github')
        git(url: 'https://${SCM_REPO_URL}', branch: '${SCM_REPO_BRANCH}', credentialsId: '${SCM_REPO_CREDS}')
      }
    }

    stage('Build and push to Harbor with Kaniko') {
      steps {
        echo 'Build and Push with Kaniko'
        container(name: 'kaniko-yaml') {
          sh '''
          /kaniko/executor --dockerfile `pwd`/Dockerfile  --context `pwd` --destination=${HARBOR_SERVER}/${HARBOR_REPO1}/${IMAGE_NAME}:${IMAGE_TAG} --destination=${HARBOR_SERVER}/${HARBOR_REPO1}/${IMAGE_NAME}:$BUILD_NUMBER --verbosity=${LOG_LEVEL} --registry-certificate ${HARBOR_SERVER}=${REGISTRY_CERT_LOC}
          '''
        }

      }
    }

  }

}
