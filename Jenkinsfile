pipeline {

  environment {
    registry = "192.168.1.8:5000/dealscandy"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/ganpaw/Movies-Finder.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "movie-finder.yaml", kubeconfigId: "k8s-jenkins-tiller-cicd-conf")
        }
      }
    }

  }

}