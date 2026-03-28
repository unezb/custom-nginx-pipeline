pipeline {
    agent any
    
    environment {
        DOCKER_HUB_CREDENTIALS_ID = 'dockerhub'
        DOCKER_HUB_REGISTRY_URL = 'https://index.docker.io/v1/'
        DOCKER_IMAGE_NAME = 'devamu/my-nginx-app'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/unezb/custom-nginx-pipeline.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build --no-cache -t ${env.DOCKER_IMAGE_NAME}:latest ."
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    withDockerRegistry(
                        credentialsId: env.DOCKER_HUB_CREDENTIALS_ID,
                        url: env.DOCKER_HUB_REGISTRY_URL
                    ) {
                        sh "docker push ${env.DOCKER_IMAGE_NAME}:latest"
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                sh "docker run -d -p 80:80 ${env.DOCKER_IMAGE_NAME}:latest"
            }
        }
    }
}