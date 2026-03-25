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
        echo "JOB_NAME=${env.JOB_NAME}"
        echo "BUILD_NUMBER=${env.BUILD_NUMBER}"
        echo "BUILD_URL=${env.BUILD_URL}"
        echo "NODE_NAME=${env.NODE_NAME}"
        echo "WORKSPACE=${env.WORKSPACE}"
        echo "GIT_URL=${env.GIT_URL}"
        echo "GIT_BRANCH=${env.GIT_BRANCH}"
        echo "BRANCH_NAME=${env.BRANCH_NAME}"
        echo "CHANGE_ID=${env.CHANGE_ID}"
        echo "CHANGE_BRANCH=${env.CHANGE_BRANCH}"
        echo "CHANGE_TARGET=${env.CHANGE_TARGET}"
        echo "GITHUB_REF=${env.GITHUB_REF}"
        echo "GITHUB_SHA=${env.GITHUB_SHA}"
        checkout scm
      }
    }

    stage('Inspect workspace') {
      steps {
        sh '''
          set -euxo pipefail
          echo "=== Shell environment snapshot ==="
          env | sort
          echo "=== Key variables ==="
          echo "JOB_NAME=${JOB_NAME:-}"
          echo "BUILD_NUMBER=${BUILD_NUMBER:-}"
          echo "BUILD_URL=${BUILD_URL:-}"
          echo "NODE_NAME=${NODE_NAME:-}"
          echo "WORKSPACE=${WORKSPACE:-}"
          echo "GIT_URL=${GIT_URL:-}"
          echo "GIT_BRANCH=${GIT_BRANCH:-}"
          echo "BRANCH_NAME=${BRANCH_NAME:-}"
          echo "CHANGE_ID=${CHANGE_ID:-}"
          echo "CHANGE_BRANCH=${CHANGE_BRANCH:-}"
          echo "CHANGE_TARGET=${CHANGE_TARGET:-}"
          echo "GITHUB_REF=${GITHUB_REF:-}"
          echo "GITHUB_SHA=${GITHUB_SHA:-}"
          pwd
          ls -la
          echo "=== Git status ==="
          git status || true
          echo "=== Git remotes ==="
          git remote -v || true
          echo "=== Git HEAD ==="
          git rev-parse HEAD || true
          echo "=== Git branches ==="
          git branch -a || true
        '''
      }
    }

    stage('Verify Docker') {
      steps {
        sh '''
          set -euxo pipefail
          echo "TEST_IMAGE=${TEST_IMAGE}"
          echo "PATH=${PATH}"
          command -v docker
          docker --version
          docker info
        '''
      }
    }

    stage('Pull test image') {
      steps {
        sh '''
          set -euxo pipefail
          echo "Pulling image: ${TEST_IMAGE}"
          docker pull "${TEST_IMAGE}"
          echo "=== Pulled image inspect ==="
          docker image inspect "${TEST_IMAGE}" --format '{{.Id}}'
          echo "=== Docker images matching test image ==="
          docker images "${TEST_IMAGE}" || true
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
