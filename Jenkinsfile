pipeline {
    agent any

    environment {
        IMAGE_NAME = 'pipeline-app'
    }

    stages {

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Run Container') {
            steps {
                bat '''
                docker stop pipeline-container || exit 0
                docker rm pipeline-container || exit 0
                docker run -d -p 3000:3000 --name pipeline-container %IMAGE_NAME%
