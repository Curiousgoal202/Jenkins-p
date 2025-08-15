pipeline {
  agent any
  environment {
    IMAGE_NAME = "demo-app"
    IMAGE_TAG  = "${env.BUILD_NUMBER}"
    CONTAINER_NAME = "demo-app"
    PORT = "80"
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
      }
    }
    stage('Stop Old Container') {
      steps {
        sh 'docker rm -f ${CONTAINER_NAME} || true'
      }
    }
    stage('Run New Container') {
      steps {
        sh 'docker run -d --name ${CONTAINER_NAME} -p ${PORT}:80 ${IMAGE_NAME}:${IMAGE_TAG}'
      }
    }
  }
  post {
    success {
      echo "App deployed: http://EC2_PUBLIC_IP:${PORT}"
    }
    cleanup {
      sh 'docker image prune -f || true'
    }
  }
}
