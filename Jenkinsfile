pipeline {
    agent any

    environment {
        IMAGE_NAME = "static-site"
        CONTAINER_NAME = "static-site-container"
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "Checking out code from GitHub..."
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh 'docker build -t ${IMAGE_NAME}:latest .'
            }
        }

        stage('Stop Old Container') {
            steps {
                echo "Stopping and removing old container (if exists)..."
                sh 'docker rm -f ${CONTAINER_NAME} || true'
            }
        }

        stage('Run New Container') {
            steps {
                echo "Starting new container..."
                sh 'docker run -d --name ${CONTAINER_NAME} -p 80:80 ${IMAGE_NAME}:latest'
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful! Access your site at http://<EC2-PUBLIC-IP>"
        }
        failure {
            echo "❌ Build or Deployment Failed. Check Jenkins logs."
        }
    }
}
