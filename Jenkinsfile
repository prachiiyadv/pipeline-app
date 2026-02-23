pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "flask-app"
        CONTAINER_NAME = "flask-app"
    }

    stages {

        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/prachiiyadv/pipeline-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat """
                    docker build -t %DOCKER_IMAGE% .
                """
            }
        }

        stage('Stop Existing Container') {
            steps {
                bat """
                    docker stop %CONTAINER_NAME% || exit 0
                    docker rm %CONTAINER_NAME% || exit 0
                """
            }
        }

        stage('Run Container') {
            steps {
                bat """
                    docker run -d -p 5000:5000 --name %CONTAINER_NAME% %DOCKER_IMAGE%
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

