node {

    stage("Git Clone"){
	
		git branch: 'main', credentialsId: 'github-credentials', url: ' https://github.com/praboworamasatrio/jenkins-kubernetes-deployment.git '
    }

    stage("Docker build"){
        sh 'docker build -t ramasatrioboy/react-app .'
        sh 'docker image list'
        sh 'docker tag ramasatrioboy/react-app ramasatrioboy/jenkins-kubernetes-deployment'
    }

	stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( ' https://registry.hub.docker.com ', registryCredential ) {
          sh 'docker push ramasatrioboy/jenkins-kubernetes-deployment'

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
 
