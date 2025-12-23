pipeline {
    agent any

    environment {
        IMAGE_NAME = "venecia1414/examen"
        DOCKER_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                  docker build -t ${IMAGE_NAME}:${DOCKER_TAG} .
                  docker tag ${IMAGE_NAME}:${DOCKER_TAG} ${IMAGE_NAME}:latest
                """
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    """
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh """
                  docker push ${IMAGE_NAME}:${DOCKER_TAG}
                  docker push ${IMAGE_NAME}:latest
                """
            }
        }
    }
}
