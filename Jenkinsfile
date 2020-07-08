pipeline {

  environment {
    registry = "192.168.1.8:5000/dealscandy"
    artifactId = "movie-finder"
    dockerImage = ""
  }

   agent{
        label "k8s-jenkins-docker-slave"
   } 

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/ganpaw/Movies-Finder.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + "/${artifactId}:latest"
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
          kubernetesDeploy(configs: "movie-finder.yaml", kubeconfigId: "cicd-manager")
        }
      }
    }

  }

}