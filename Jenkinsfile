pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials') // Docker Hub credentials stored in Jenkins
        DOCKER_HUB_REPO = 'your-dockerhub-username' // Replace with your DockerHub username
    }

    stages {
        stage('Build Docker Images') {
            steps {
                script {
                    sh 'docker build -t ${DOCKER_HUB_REPO}/backend ./backend'
                    sh 'docker build -t ${DOCKER_HUB_REPO}/frontend ./frontend'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh 'echo ${DOCKER_HUB_CREDENTIALS_PSW} | docker login -u ${DOCKER_HUB_CREDENTIALS_USR} --password-stdin'
                    sh 'docker push ${DOCKER_HUB_REPO}/backend'
                    sh 'docker push ${DOCKER_HUB_REPO}/frontend'
                }
            }
        }

        stage('Deploy Locally') {
            steps {
                script {
                    // Gracefully shut down existing containers
                    sh 'docker-compose down || true'
                    // Start the new deployment
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        always {
            script {
                echo "Pipeline execution completed."
            }
        }
        failure {
            script {
                echo "Pipeline execution failed. Check logs for details."
            }
        }
    }
}
