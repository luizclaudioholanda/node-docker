pipeline{  
  environment {
    registry = "luizholandadocker/node-docker"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any

    tools { nodejs "node" }

    stages {
        stage('Build'){
           steps{
              script{
                sh 'npm install'
              } 
           }   
        }
        stage('Building image') {
            steps{
                script {
                  dockerImage = docker.build registry + ":latest"
                 }
             }
          }
          stage('Push Image') {
              steps{
                  script {
                       docker.withRegistry( '', registryCredential){                            
                       dockerImage.push()
                      }
                   }
                } 
           }
           stage('Deploying into k8s'){
              steps{
                withKubeConfig([credentialsId: 'minikube']) {
                  sh 'kubectl apply -f deployment.yaml'
                  sh "kubectl rollout history deployment nodejs-deployment"
                }
              }
        }
    }
}