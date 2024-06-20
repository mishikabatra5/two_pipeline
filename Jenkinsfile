pipeline {
    agent any
    environment {
        CI = 'true'
        DOCKER_IMAGE = 'my-nginx-website'
        DOCKER_TAG = 'latest'
        REGISTRY_CREDENTIALS = 'a782c2bc-47ea-4929-b865-3e8488b8e134'  // Jenkins credential ID for Docker Hub
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', "${REGISTRY_CREDENTIALS}") {
                        dockerImage.push()
                    }
                }
            }
        }
    }
    post {
        always {
            script {
                sh "docker rmi ${DOCKER_IMAGE}:${DOCKER_TAG}"
            }
        }
    }
}
