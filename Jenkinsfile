pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "petrobubka/my_gogs_image:${BUILD_NUMBER}"
        STACK_NAME = "myapp"
    }
    stages {
        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Build and push Docker image
                    sh '''
                    docker build -t ${DOCKER_IMAGE} .
                    docker push ${DOCKER_IMAGE}
                    '''
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                script {
                    // Update image in docker-compose.yml dynamically
                    sh "sed -i 's|petrobubka/my_gogs_image:16|${DOCKER_IMAGE}|g' docker-compose.yml"

                    // Deploy or update the stack
                    sh "docker stack deploy -c docker-compose.yml ${STACK_NAME}"
                }
            }
        }
    }
    post {
        always {
            // Perform any cleanup or final steps
            echo "Build and deployment processes are completed."
        }
    }
}
