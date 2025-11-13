pipeline {
  agent any
  
   tools {nodejs "node"}
    
  stages {
    stage("Clone code from GitHub") {
            steps {
                script {
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GITCREDS', url: 'https://github.com/Malli120812/Deploy-NodeApp-to-AWS-EKS-using-Jenkins-Pipeline']])
                }
            }
        }
     
    stage('Node JS Build') {
      steps {
        sh 'npm install'
      }
    }
  
     stage('Build Node JS Docker Image') {
            steps {
                script {
                  sh 'docker build -t mallikarjuna1208/dotnet .'
                }
            }
        }


        stage('Deploy Docker Image to DockerHub') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'DOCKERCREDS', variable: 'DOCKERCREDS')]) {
                    sh 'docker login -u mallikarjuna1208 -p ${DOCKERCREDS}'
            }
            sh 'docker push mallikarjuna1208/dotnet'
        }
            }   
        }
         
     stage('Deploying Node App to Kubernetes') {
      steps {
        script {
          sh ('aws eks update-kubeconfig --name dev-project --region ap-south-1')
          sh "kubectl get ns"
          sh "kubectl apply -f nodejsapp.yaml"
        }
      }
    }

  }
}
