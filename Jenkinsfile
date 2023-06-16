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
	
    stage("SSH Into k8s Server") {
        def remote = [:]
        remote.name = 'boy-VirtualBox'
        remote.host = '192.168.1.20'
        remote.user = 'boy'
        remote.password = '100%h4l4l'
        remote.allowAnyHosts = true

        stage('Put k8s-spring-boot-deployment.yml onto k8smaster') {
            sshPut remote: remote, from: 'deployment.yaml', into: '.'
        }

        stage('Deploy spring boot') {
          sshCommand remote: remote, command: "kubectl apply -f deployment.yaml"
        }
    }
  }
}
 
