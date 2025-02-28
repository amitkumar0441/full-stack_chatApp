pipeline {
    agent any 

    environment {
        IMAGE_TAG = "${BUILD_NUMBER}" // Using BUILD_NUMBER for versioning
        KUBECONFIG = '/home/masterserver/.kube/config'  // Set the path for kubectl config
    }

    stages {
        stage('Stage 01 - Checkout the Code') {
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

        stage('Stage 04 - Deploy to Kubernetes') {
            steps {
                script {
                    dir('k8s') {
                        sh """
                            kubectl apply -f namespace.yml &&
                            kubectl apply -f mongodb-pv.yml &&
                            kubectl apply -f mongodb-pvc.yml &&
                            kubectl apply -f secrets.yml &&
                            kubectl apply -f mongodb-deployment.yml &&
                            kubectl apply -f mongodb-service.yml &&
                            kubectl apply -f backend-deployment.yml &&
                            kubectl apply -f backend-service.yml &&
                            kubectl apply -f frontend-deployment.yml &&
                            kubectl apply -f frontend-service.yml
                        """
                    }
                }
            }
        }
    }
}
