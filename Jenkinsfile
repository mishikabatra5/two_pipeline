pipeline {
    agent any
    environment {
        REPO_NAME = 'mishikabatra/my_nginx_image'
        DOCKERHUB_CREDENTIALS = 'a782c2bc-47ea-4929-b865-3e8488b8e134'
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    echo "Branch: ${env.BRANCH_NAME}"
                    checkout scm
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def imageTag = "${env.BRANCH_NAME}-${env.BUILD_ID}"
                    def image = docker.build("${env.REPO_NAME}:${imageTag}", '-f Dockerfile .')

                    docker.withRegistry('https://registry.hub.docker.com/', DOCKERHUB_CREDENTIALS) {
                        image.push()
                    }
                }
            }
        }
    }
    post {
        success {
            script {
                echo "Successfully built and pushed the Docker image for branch: ${env.BRANCH_NAME}"
            }
        }
        failure {
            script {
                echo "Failed to build and push the Docker image for branch: ${env.BRANCH_NAME}"
            }
        }
    }
}
