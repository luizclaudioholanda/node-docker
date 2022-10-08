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
                  dockerImage = docker.build("luizholandadocker/node-docker")
                 }
             }
        }
        stage('Push Image') {
            steps{
                script {
                      docker.withRegistry( '', registryCredential){                            
                      dockerImage.push("${env.BUILD_NUMBER}")
                    }
                  }
              } 
        }
        stage('Update Manifest') {
          steps {
            build job:'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
          }
        }
        // stage('Deploying into k8s'){
        //   steps{
        //     withKubeConfig([credentialsId: 'minikube']) {
        //       sh 'kubectl apply -f deployment.yaml'
        //     }
        //   }
        // }
    }
}