pipeline {
    agent none  // Define no global agent; use specific agents per stage
    environment {
        DOCKER_IMAGE = 'petrobubka/my_gogs_image:${BUILD_NUMBER}'
    }
    stages {
        stage('Setup Environment') {
            agent { docker { image 'alpine:3.15' } }
            steps {
                sh '''
                echo -e "https://alpine.global.ssl.fastly.net/alpine/v3.18/community" > /etc/apk/repositories
                echo -e "https://alpine.global.ssl.fastly.net/alpine/v3.18/main" >> /etc/apk/repositories
                apk update
                apk add --no-cache binutils go postgresql-client git openssh
                '''
            }
        }

        stage('Build Gogs') {
            agent { docker { image 'alpine:3.15' } }
            steps {
                sh 'go build -o gogs -buildvcs=false'
            }
        }

        stage('Test Gogs') {
            agent { docker { image 'alpine:3.15' } }
            steps {
                sh 'go test -v -cover ./...'
            }
        }

        stage('Build & Push Docker Image') {
            agent { docker { image 'docker:19.03' } }
            steps {
                script {
                    sh '''
                    docker build -t ${DOCKER_IMAGE} .
                    docker push ${DOCKER_IMAGE}
                    '''
                }
            }
        }

        stage('Deploy App using Docker Compose') {
            agent { docker { image 'docker/compose:1.29.2' } }
            steps {
                script {
                    // Assuming you have a docker-compose.yml file configured for deployment
                    sh '''
                    sed -i "s/petrobubka\\/my_gogs_image:16/${DOCKER_IMAGE}/" docker-compose.yml
                    docker-compose up -d
                    '''
                }
            }
        }
    }
}
