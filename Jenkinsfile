pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'omarelk18/nodejschatapp'
        GIT_REPO = 'https://github.com/Omarelk17/NodejsChatApp'
        DOCKER_USERNAME = credentials('docker-username-id') // Update with your credentials ID
        DOCKER_PASSWORD = credentials('docker-password-id') // Update with your credentials ID
    }

    stages {
        stage('Checkout SCM') {
            steps {
                echo 'Checking out source code from GitHub Repository...'
                checkout scm
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    echo 'Logging in to Docker Hub...'
                    sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
                }
            }
        }

        stage('Build and Tag Docker Image with Compose') {
            steps {
                script {
                    echo 'Building and Tagging Docker Image with Docker Compose...'
                    sh "ls"
                    sh "docker compose -f docker-compose.yml build app" 
                    sh "docker tag nodejschatapp ${DOCKER_IMAGE}:latest"
                    sh "docker tag nodejschatapp ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    echo 'Pushing Docker Image to Docker Hub...'
                    sh "docker push ${DOCKER_IMAGE}:latest"
                    sh "docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                }
            }
        }

        stage('Run Application with Docker Compose') {
            steps {
                script {
                    echo 'Pulling and Running Docker Image with Docker Compose...'
                    sh "docker pull ${DOCKER_IMAGE}:latest"
                    sh "docker compose -f docker-compose.yml up -d"
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up Docker resources...'
            // Uncomment to clean up
            // sh "docker compose -f docker-compose.yml down"
            // sh "docker system prune -f"
        }
    }
}
