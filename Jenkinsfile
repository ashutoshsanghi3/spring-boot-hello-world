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
        withCredentials([usernamePassword(credentialsId: "${REGISTRY_CREDENTIALS}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
          script {
            def imageTag = "${DOCKER_IMAGE}:${env.BUILD_NUMBER}"
            sh """
              docker build -t ${imageTag} .
              echo $PASSWORD | docker login -u $USERNAME --password-stdin
              docker push ${imageTag}
            """
          }
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
