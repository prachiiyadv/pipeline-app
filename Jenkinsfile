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

        stage('Trivy Scan') {
            steps {
                bat """
                    echo Scanning Docker image for vulnerabilities...
                    trivy image --exit-code 1 --severity HIGH,CRITICAL %DOCKER_IMAGE%
                    trivy image -f template --template "@contrib/html.tpl" -o trivy-report.html %DOCKER_IMAGE%
                    echo Trivy scan completed.
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
            echo 'Pipeline completed successfully! Docker image built, scanned, and deployed.'
        }
        failure {
            echo 'Pipeline failed. Check logs and Trivy report.'
        }
    }
}
