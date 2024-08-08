pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('DockerPat')
        DOCKER_IMAGE = 'goodnessmark/frontend'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the repository from GitHub
                git url: 'https://github.com/Goodyoma/BOLE_FESTIVAL.git', branch: 'master'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image using the Dockerfile in the repository
                    docker.build("${env.DOCKER_IMAGE}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DockerPat') {
                        def dockerImage = docker.image("${env.DOCKER_IMAGE}:${env.BUILD_ID}")
                        
                        // Tag the image as 'latest'
                        dockerImage.tag('latest')
                        
                        // Push the image with the build ID tag and latest tag
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up the workspace after the build
            cleanWs()
        }
    }
}
