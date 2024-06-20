pipeline {
    agent any
    environment {
        CI = 'true'
        DOCKER_IMAGE = 'my_nginx_image'
        DOCKER_TAG = 'latest'
        REGISTRY_CREDENTIALS = 'a782c2bc-47ea-4929-b865-3e8488b8e134'  // Jenkins credential ID for Docker Hub
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the Git repository
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    def app = docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }
        stage('Test Docker Image') {
            steps {
                script {
                    // Run the Docker container to test if it starts correctly
                    docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").inside {
                        sh 'echo "Container started successfully"'
                    }
                }
            }
        }
        stage('Deliver for Development') {
            when {
                branch 'development'
            }
            steps {
                script {
                    // Push the Docker image to the registry for development
                    docker.withRegistry('', "${REGISTRY_CREDENTIALS}") {
                        docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push("development")
                    }
                }
            }
        }
        stage('Deploy for Production') {
            when {
                branch 'production'
            }
            steps {
                script {
                    // Push the Docker image to the registry for production
                    docker.withRegistry('', "${REGISTRY_CREDENTIALS}") {
                        docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push("production")
                    }
                }
            }
        }
    }
    post {
        always {
            // Clean up the Docker image from the local Docker daemon
            script {
                sh "docker rmi ${DOCKER_IMAGE}:${DOCKER_TAG}"
            }
        }
    }
}
