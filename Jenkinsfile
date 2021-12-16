pipeline {
  agent {
    node {
      label 'kaniko-harbor'
    }


  }
  stages {
    stage('checkout SCM') {
      steps {
        // git(url: 'https://github.com/j-kyle-moore/jenkins-bo-reg1-apache2.git', branch: 'master', credentialsId: 'jkm-github')
        git(url: 'https://github.com/j-kyle-moore/jenkins-bo-reg1-apache2.git', branch: 'master', credentialsId: 'jkm-github')
      }
    }

    stage('Build and push to Harbor with Kaniko') {
      steps {
        echo 'Build and Push with Kaniko'
        container(name: 'kaniko-harbor') {
          sh '''
          /kaniko/executor --dockerfile `pwd`/Dockerfile  --context `pwd` --destination=${HARBOR_SERVER}/${HARBOR_REPO1}/${IMAGE_NAME}:${IMAGE_TAG} --destination=${HARBOR_SERVER}/${HARBOR_REPO1}/${IMAGE_NAME}:$BUILD_NUMBER --verbosity=${LOG_LEVEL} --registry-certificate ${HARBOR_SERVER}=${REGISTRY_CERT_LOC}
          '''
        }
      }
    }

    // stage('Scan with Anchore') {
    //         steps {
    //             sh 'echo "$HARBOR_SERVER/$HARBOR_REPO1/$IMAGE_NAME" ${WORKSPACE}/Dockerfile > anchore_images'
    //             anchore 'anchore_images'
    //         }
    //     }
    //
    // stage('Send Email with Build Results') {
    //       steps {
    //       	emailext attachLog: true, body: "Build URL: ${RUN_DISPLAY_URL}\n \n Artifacts URL: ${RUN_ARTIFACTS_DISPLAY_URL}\n \n Jenkins Build URL (with Anchore Scan Results): https://${JENKINS_SERVER}/job/${JENKINS_PIPELINE_NAME}/lastCompletedBuild/anchore-results/\n \n Harbor Repo (with Trivy Scan Results): https://${env.HARBOR_SERVER}/harbor/projects", subject: "${BUILD_TAG}", to: "${env.EMAIL_RECPTS}"
    //         // emailext attachLog: true, body: "test", subject: "test", to: "${env.EMAIL_RECPTS}"
    //   }
    // }
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
