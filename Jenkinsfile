pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'omarelk18/nodejschatapp'
        GIT_REPO = 'https://github.com/Omarelk17/NodejsChatApp'
    }

    stages {
        stage('Clone and Build') {
            steps {
                script {
                    echo 'Cloning GitHub Repository and Building Docker Image...'
                    // Clone the repository
                    git branch: 'main', url: "${env.GIT_REPO}"

                    // Build the Docker image
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
                    withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        // Log in to Docker Hub
                        sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"

                        // Push images to Docker Hub
                        sh "docker push ${DOCKER_IMAGE}:latest"
                        sh "docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                    }
                }
            }
        }

        stage('Run Application') {
            steps {
                script {
                    echo 'Pulling and Running Docker Image with Docker Compose...'
                    sh "docker compose -f docker-compose.yml pull" // Ensure the latest image is pulled
                    sh "docker compose -f docker-compose.yml up -d" // Run in detached mode
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    echo 'Cleaning up Docker resources...'
                    // Optionally clean up unused resources
                    sh "docker system prune -f"
                }
            }
        }
    }
}
