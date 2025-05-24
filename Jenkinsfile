pipeline {
  agent any
  environment {
    DOCKER_IMAGE_NAME = 'ashutoshsanghi/java-springboot'
    DOCKER_HUB_CREDENTIALS = 'docker-cred' // Your Jenkins credentials ID for Docker Hub
  }
  stages {
    stage('Build & Test') {
      steps {
        script {
          sh 'mvn clean install'
        }
      }
    }
    stage('Login to Docker Hub') {
      steps {
        script {
          // Login to Docker Hub using Jenkins credentials
          withCredentials([usernamePassword(credentialsId: DOCKER_HUB_CREDENTIALS, passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                sh "echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin"
            }
        }
      }
    }
    stage('Build Docker Image') {
      steps {
        script {
          // Build the Docker image
         sh "docker build -t ${DOCKER_IMAGE_NAME}:latest ."
        }
      }
    }
    stage('Push Docker Image') {
      steps {
        script {
          // Push the Docker image to Docker Hub
          sh "docker push ${DOCKER_IMAGE_NAME}:latest"
        }
      }
    }
    stage('Kubernetes Deployment and Service') {
      steps {
        script {
          // Apply the Kubernetes Deployment file
          sh "kubectl apply -f deployment.yaml"
        }
      }
    }
  }
}
