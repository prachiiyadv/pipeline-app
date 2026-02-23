pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "flask-app"
        CONTAINER_NAME = "flask-app"
    }

    stages {

        stage('Clone Repo') {
            steps {
                // Pull code from GitHub
                // Replace 'main' with your branch name if different
                git branch: 'main', url: 'https://github.com/prachiiyadv/pipeline-app.git'
                // If repo is private, add: credentialsId: 'your-credential-id'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build Docker image
                sh """
                    docker build -t ${DOCKER_IMAGE} .
                """
            }
        }

        stage('Stop Existing Container') {
            steps {
                // Stop old container if running
                sh """
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                """
            }
        }

        stage('Run Container') {
            steps {
                // Run new container
                sh """
                    docker run -d -p 5000:5000 --name ${CONTAINER_NAME} ${DOCKER_IMAGE}
                """
            }
        }

    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs.'
        }
    }
}
