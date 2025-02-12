pipeline {
    agent any

    environment {
        GIT_CREDENTIALS_ID = 'github-token' 
        IMAGE_NAME = 'flask-app'
        CONTAINER_NAME = 'flask-container'
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    git branch: 'main',
                        credentialsId: "${GIT_CREDENTIALS_ID}",
                        url: 'https://github.com/Nisha-Velmurugan/flask-app.git'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${IMAGE_NAME}:latest .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh '''
                    if [ $(docker ps -q -f name=${CONTAINER_NAME}) ]; then
                        docker stop ${CONTAINER_NAME}
                        docker rm ${CONTAINER_NAME}
                    fi

                    docker run -d -p 5000:5000 --name ${CONTAINER_NAME} ${IMAGE_NAME}:latest
                    '''
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    sh 'docker system prune -f'
                }
            }
        }
    }

    post {
        success {
            echo "Deployment Successful: Flask app is running."
        }
        failure {
            echo "Deployment Failed!"
        }
    }
}
