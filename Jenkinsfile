pipeline {
  environment {
    dockerimagename = "santhoshk8s/react-app"
    dockerImage = ""
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
    stage('Pushing Image') {
      environment {
          registryCredential = 'docker_account'
           }
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
            steps {
                script {
                    // Load Kubernetes DSL
                    def k8s = load 'deployment.yaml'

                    // Set Minikube credentials
                    withCredentials([usernamePassword(credentialsId: 'minikube-credentials', usernameVariable: 'MINIKUBE_USERNAME', passwordVariable: 'MINIKUBE_PASSWORD')]) {
                        // Apply deployment
                        sh "kubectl apply -f deployment.yaml"
                    }

                    // Load and apply service
                    sh "kubectl apply -f service.yaml"
                }
            }
        }
    }
}