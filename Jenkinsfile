pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'uanike/web-app'           // Docker Hub repo name
        DOCKER_TAG = 'latest'
        REMOTE_USER = 'anick'
        REMOTE_HOST = '172.31.129.66'
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "$DOCKER_CREDENTIALS_ID", 
                                                  usernameVariable: 'DOCKER_USER', 
                                                  passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push $DOCKER_IMAGE:$DOCKER_TAG
                    '''
                }
            }
        }

        stage('Deploy to Remote Host') {
            steps {
                sshagent(['remote-host-ssh-key']) {
                    sh '''
                    ssh $REMOTE_USER@$REMOTE_HOST "docker pull $DOCKER_IMAGE:$DOCKER_TAG"
                    ssh $REMOTE_USER@$REMOTE_HOST "docker rm -f my-web-app || true"
                    ssh $REMOTE_USER@$REMOTE_HOST "docker run -d --name my-web-app -p 8080:3000 $DOCKER_IMAGE:$DOCKER_TAG"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully!'
        }
        failure {
            echo 'Deployment failed. Check the logs!'
        }
    }
}
