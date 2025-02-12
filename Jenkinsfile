pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-app"
        CONTAINER_NAME = "flask-container"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/your-username/flask-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop any running container with the same name
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"

                    // Run a new container
                    sh "docker run -d --name ${CONTAINER_NAME} -p 5000:5000 ${IMAGE_NAME}"
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    sh "docker system prune -f"
                }
            }
        }
    }
}
