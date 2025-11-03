pipeline {
  agent any
  environment { DOCKERHUB_REPO = "junaidh1331/Mywebapp" }
  stages {
    stage('Checkout'){ steps { checkout scm } }
    stage('Build'){
      steps { sh 'docker --version || true'; sh "docker build -t ${DOCKERHUB_REPO}:latest ." }
    }
    stage('Push'){
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
          sh 'docker tag ${DOCKERHUB_REPO}:latest ${DOCKERHUB_REPO}:${BUILD_NUMBER}'
          sh 'docker push ${DOCKERHUB_REPO}:${BUILD_NUMBER}'
          sh 'docker push ${DOCKERHUB_REPO}:latest || true'
        }
      }
    }
  }
}