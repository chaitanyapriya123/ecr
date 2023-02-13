pipeline {
    agent any
    environment {
        registry = "acct_id.dkr.ecr.us-east-2.amazonaws.com/your_ecr_repo"
    }
   
    stages {
        stage('Cloning Git') {
            steps {
                  checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/chaitanyapriya123/ecr.git']])
            }
        }
  
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry
        }
      }
    }
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
         script {
                sh 'aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin acct_id.dkr.ecr.us-east-2.amazonaws.com'
                sh 'docker push acct_id.dkr.ecr.us-east-2.amazonaws.com/your_ecr_repo:latest'
         }
        }
      }
   
         // Stopping Docker containers for cleaner Docker run
     stage('stop previous containers') {
         steps {
            sh 'docker ps -f name=latest -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=latest -q | xargs -r docker container rm'
         }
       }
      
    stage('Docker Run') {
     steps{
         script {
                sh 'docker run -d -p 8096:5000 --rm latest acct_id.dkr.ecr.us-us-east-2.amazonaws.com/your_ecr_repo:latest'
            }
      }
    }
    }
}
