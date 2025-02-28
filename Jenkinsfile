pipeline {
    agent any 

    environment {
        IMAGE_TAG = "${BUILD_NUMBER}" // Using BUILD_NUMBER for versioning
    }

    stages {
        stage('stage01-Checkout the code') {
            steps {
                git branch: 'main', url: 'https://github.com/amitkumar0441/full-stack_chatApp.git'
            }
        }

        stage('stage02-build the docker images') {
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
    }
}
