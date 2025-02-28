pipeline {
    agent any 

    environment {
        IMAGE_TAG = "${BUILD_NUMBER}" // Using BUILD_NUMBER for versioning
    }

    stages {
        stage('Stage 01 - Checkout the code') {
            steps {
                git branch: 'main', url: 'https://github.com/amitkumar0441/full-stack_chatApp.git'
            }
        }

        stage('Stage 02 - Build the Docker Images') {
            steps {
                script {
                    dir('frontend') {
                        sh "docker build -t amitkumar0441/chatapp-frontend:${IMAGE_TAG} ."
                    }
                    dir('backend') {
                        sh "docker build -t amitkumar0441/chatapp-backend:${IMAGE_TAG} ."
                    }
                }
            }
        }

        stage('Stage 03 - Push Docker Images to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockercredentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    script {
                        sh """
                            docker login -u '${DOCKER_USERNAME}' -p '${DOCKER_PASSWORD}' &&
                            docker push amitkumar0441/chatapp-frontend:${IMAGE_TAG} &&
                            docker push amitkumar0441/chatapp-backend:${IMAGE_TAG} &&
                            docker logout &&
                            docker rmi amitkumar0441/chatapp-frontend:${IMAGE_TAG} && 
                            docker rmi amitkumar0441/chatapp-backend:${IMAGE_TAG}
                        """
                    }
                }
            }
        }
    }
}
