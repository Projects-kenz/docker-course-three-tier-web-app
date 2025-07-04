pipeline {
 agent {
  docker {
    image 'ghcr.io/catthehacker/ubuntu:act-latest'
    args '-v /var/run/docker.sock:/var/run/docker.sock'
  }
}

  environment {
    DOCKER_CONFIG = "${WORKSPACE}/.docker"
    IMAGE_FRONTEND = "kenzieeiy/python-three-tier:${BUILD_NUMBER}"
     IMAGE_BACKEND = "kenzieeiy/python-three-tier:${BUILD_NUMBER}"
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main',
            url: 'https://github.com/Projects-kenz/docker-course-three-tier-web-app.git'
      }
    }

  
    stage('Build Docker Image') {
      steps {
        script {
          sh "docker compose build"
        }
      }
    }

  stage('Tag and Push to Docker Hub') {
      steps {
        script {
          // Tag backend and frontend individually
          sh """
            docker tag docker-course-three-tier-web-app_backend ${IMAGE_BACKEND}-backend
            docker tag docker-course-three-tier-web-app_frontend ${IMAGE_FRONTEND}-frontend
          """

          docker.withRegistry('https://index.docker.io/v1/', 'docker-cred') {
            sh "docker push ${IMAGE_BACKEND}-backend"
            sh "docker push ${IMAGE_FRONTEND}-frontend"
          }
        }
      }
    }
  }
}
