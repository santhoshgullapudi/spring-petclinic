pipeline {
    agent any
    
    environment {
        // Define your Docker registry credentials here
        AWS_DEFAULT_REGION = 'ap-south-1'
        AWS_ACCOUNT_ID = '381492106362'
        ECR_REPOSITORY = 'docker-image'
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout your Git repository
                git 'https://github.com/santhoshgullapudi/spring-petclinic.git'
            }
        }
        
        stage('Build and Push Docker Image') {
            steps {
                script {
                    def dockerImage = docker.build("spring-petclinic")

                    // Login to Amazon ECR
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'your-aws-credentials-id', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                        docker.withRegistry("https://${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com", 'ecr:us-east-1') {
                            // Push the Docker image to ECR
                            dockerImage.push("${ECR_REPOSITORY}:latest")
                        }
                    }
                }
            }
        }
    }
}