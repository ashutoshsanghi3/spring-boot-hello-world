pipeline {
  agent any

  environment {
    DOCKER_IMAGE = "ashutoshsanghi/mavenimg"
    REGISTRY_CREDENTIALS = "dockerhub-credentials-id"
  }

  stages {
    stage('Build and Test') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Docker Build and Push') {
      when {
        branch 'main'
      }
      steps {
        script {
          dockerImage = docker.build("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
        }
        withCredentials([usernamePassword(credentialsId: "${REGISTRY_CREDENTIALS}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
          sh "echo $PASSWORD | docker login -u $USERNAME --password-stdin"
          sh "docker push ${DOCKER_IMAGE}:${env.BUILD_NUMBER}"
        }
      }
    }

    stage('Deploy to Kubernetes') {
      when {
        branch 'main'
      }
      steps {
        sh 'kubectl apply -f k8s/deployment.yaml'
        sh 'kubectl apply -f k8s/service.yaml'
      }
    }
  }

  post {
    always {
      echo "Pipeline execution complete"
    }
  }
}
