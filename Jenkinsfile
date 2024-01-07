pipeline {
  environment {
    dockerimagename = "bravinwasike/react-app"
    dockerImage = ""
    registryCredential = 'Docker-Hub-Creds'
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
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')

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
