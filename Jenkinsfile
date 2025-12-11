pipeline {
    agent any

    environment {
        // Replace with your Docker Hub username/repo
        DOCKER_IMAGE = 'uanike/web-app'
        DOCKER_CREDENTIALS_ID = 'uanike' // Jenkins credentials ID
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Checking out code..."
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                // Use Windows bat command to build Docker image
                bat "docker build -t %DOCKER_IMAGE% ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo "Pushing Docker image to Docker Hub..."
                // Use Jenkins credentials to login and push
                withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat "docker login -u %DOCKER_USER% -p %DOCKER_PASS%"
                    bat "docker push %DOCKER_IMAGE%"
                }
            }
        }
    }

    post {
        success {
            echo "Docker image successfully built and pushed!"
        }
        failure {
            echo "Pipeline failed. Check console output for errors."
        }
    }
}
