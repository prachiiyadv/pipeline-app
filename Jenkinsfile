pipeline {
    agent any

    environment {
        IMAGE_NAME = 'pipeline-app'
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/prachiiyadv/pipeline-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop pipeline-container || true
                docker rm pipeline-container || true
                docker run -d -p 3000:3000 --name pipeline-container $IMAGE_NAME
                '''
            }
        }
    }
}
