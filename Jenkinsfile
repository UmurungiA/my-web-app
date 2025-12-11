pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'anick/my-web-app'
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image..."
                    dockerImage = docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    echo "Pushing image to Docker Hub..."
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        dockerImage.push('latest')
                    }
                }
            }
        }

    }
}
