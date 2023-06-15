pipeline {
  environment {
    dockerimagename = "ramasatrioboy/react-app"
    dockerImage = ""
  }
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git branch: 'main', credentialsId: 'github-credentials', url: ' https://github.com/praboworamasatrio/jenkins-kubernetes-deployment.git '
      }
    }
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( ' https://registry.hub.docker.com ', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }
    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
		  kubectl apply -f deployment.yaml -n default
		  kubectl apply -f service.yaml -n default
        }
      }
    }
  }
}
