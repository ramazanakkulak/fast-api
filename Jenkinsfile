pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = "my-fastapi-app"
        DOCKER_TAG = "v1"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Docker imajını build et
                    sh "docker build -t ${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER} -f backend-service.dockerfile ."
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    // Docker imajını tag'le
                    sh "docker tag ${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER} ${DOCKER_IMAGE_NAME}:${DOCKER_TAG}"
                }
            }
        }
    }

    post {
        success {
            echo 'Docker image build and tagging succeeded!'
        }
        failure {
            echo 'Docker image build or tagging failed!'
        }
    }
}
