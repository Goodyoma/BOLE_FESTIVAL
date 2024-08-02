pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('DockerPat')
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Goodyoma/BOLE_FESTIVAL.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t goodnessmark/frontend:8 .'
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://hub.docker.com/', 'DockerPat') {
                        sh 'docker push goodnessmark/frontend:8'
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}

