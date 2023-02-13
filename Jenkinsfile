pipeline {
    agent any
    stages {
        stage('ECR login') {
            steps {
                script{
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 210658836212.dkr.ecr.us-east-1.amazonaws.com'
                }
            }
        }
        stage('Creating Image') {
            steps {
                script{
                sh 'echo FROM httpd:2.4 >> Dockerfile'
                }
            }
        }
         stage('Build and tag image') {
            steps {
                script{
                sh ''' docker build -t task .
                docker tag task:latest 210658836212.dkr.ecr.us-east-1.amazonaws.com/task:latest'''
                }
            }
        }
         stage('Push image ') {
            steps {
                script{
                sh 'docker push 210658836212.dkr.ecr.us-east-1.amazonaws.com/task:latest'
                }
            }
        }
    }
}
