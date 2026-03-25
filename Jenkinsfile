pipeline {
  agent any

  options {
    timestamps()
  }

  environment {
    TEST_IMAGE = 'ghcr.io/star-bnl/star-spack:v0.3.0-root5-gcc485'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Inspect workspace') {
      steps {
        sh '''
          set -euxo pipefail
          pwd
          ls -la
        '''
      }
    }

    stage('Verify Docker') {
      steps {
        sh '''
          set -euxo pipefail
          docker --version
          docker info >/dev/null
        '''
      }
    }

    stage('Pull test image') {
      steps {
        sh '''
          set -euxo pipefail
          docker pull "${TEST_IMAGE}"
          docker image inspect "${TEST_IMAGE}" --format '{{.Id}}'
        '''
      }
    }
  }

  post {
    always {
      echo "Build finished: ${currentBuild.currentResult}"
    }
  }
}
