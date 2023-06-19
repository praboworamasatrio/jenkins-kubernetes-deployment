pipeline {
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git 'https://github.com/praboworamasatrio/jenkins-kubernetes-deployment.git'
      }
    }
    stage('Build image') {
      steps{
		sh 'docker build -t ramasatrioboy/react-app .'
      }
    }
  }
}
