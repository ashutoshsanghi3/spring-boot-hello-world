pipeline {
  agent any
  stages {       
    stage('Kubernetes Deployment and Service') {
      steps {
        script {
         
          // Apply the Kubernetes Service file
          sh "kubectl apply -f service.yaml"
        }
      }
    }
  }
}
