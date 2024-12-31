
pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
        DOCKER_HUB_REPO = 'your-dockerhub-username'
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo/ci-cd-project.git'
            }
        }
        stage('Build Docker Images') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_HUB_REPO/backend ./backend'
                    sh 'docker build -t $DOCKER_HUB_REPO/frontend ./frontend'
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    sh 'docker login -u $DOCKER_HUB_CREDENTIALS_USR -p $DOCKER_HUB_CREDENTIALS_PSW'
                    sh 'docker push $DOCKER_HUB_REPO/backend'
                    sh 'docker push $DOCKER_HUB_REPO/frontend'
                }
            }
        }
        stage('Deploy Locally') {
            steps {
                script {
                    sh 'docker-compose down || true'
                    sh 'docker-compose up -d'
                }
            }
        }
    }
}