pipeline {
    agent any

    environment {
        IMAGE_NAME = 'favorites-node'
        CONTAINER_NAME = 'favorites-cont'
        DOCKER_NETWORK = 'favorites-net'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Create Docker Network') {
            steps {
                script {
                    // Create the Docker network if it doesn't exist
                    sh "docker network inspect ${DOCKER_NETWORK} || docker network create ${DOCKER_NETWORK}"
                }
            }
        }

       

        stage('Run Application Container') {
            steps {
                script {
                    // Run the application container
                    sh "docker run --name ${CONTAINER_NAME} -d --rm -p 3003:3000 --network ${DOCKER_NETWORK} ${IMAGE_NAME}"
                }
            }
        }
    }

    post {
        always {
            // Clean up unused Docker resources
            sh "docker system prune -f"
        }
    }
}
