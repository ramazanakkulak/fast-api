pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = "my-fastapi-app"
        DOCKER_TAG = "v1"
        DOCKER_REPOSITORY = "selftuts.local.com:5000/ramazan"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER} -f backend-service.dockerfile ."
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    sh "docker tag ${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER} ${DOCKER_IMAGE_NAME}:${DOCKER_TAG}"
                    sh "docker push ${DOCKER_REPOSITORY}/${DOCKER_IMAGE_NAME}:${DOCKER_TAG}"
                }
            }
        }
        stage('Deploy k8s') {
            steps {
                
                withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'master', contextName: '', credentialsId: 'SECRET_TOKEN', namespace: 'default', serverUrl: 'https://192.168.10.9:6443']]) {
                sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/v1.20.5/bin/linux/amd64/kubectl"'  
                sh 'chmod u+x ./kubectl'  
                sh './kubectl apply -f fastapi-service.yaml'
                sh './kubectl apply -f fastapi-deployment.yaml'
        }
      }
    }
  }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
