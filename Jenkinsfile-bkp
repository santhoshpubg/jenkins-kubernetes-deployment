pipeline {
  environment {
    dockerimagename = "bravinwasike/react-app"
    dockerImage = ""
    registryCredential = 'docker_account'
    DOCKER_REPO = 'santhoshk8s/k8s'
    DOCKER_TAG = 'latest'
  }
  agent any
  stages {
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
  stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
          docker.image("${DOCKER_REPO}:${DOCKER_TAG}").push()
          }
        }
      }
    }
    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml","service.yaml")
        }
      }
    }
  }
}
