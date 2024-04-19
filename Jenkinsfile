pipeline {
    agent none // Use 'none' because we specify the environment for each stage

    stages {
        stage('Preparation') {
            agent {
                docker {
                    image 'alpine:3.15'
                }
            }
            stages {
                stage('Install dependencies') {
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
                    steps {
                        sh 'go build -o gogs -buildvcs=false'
                    }
                }

                stage('Test Gogs') {
                    steps {
                        sh 'go test -v -cover ./...'
                    }
                }
            }
        }

        stage('Kaniko Build & Push Image') {
            agent {
                docker {
                    image 'gcr.io/kaniko-project/executor:debug'
                    args '--entrypoint=""'
                }
            }
            environment {
                DOCKER_CONFIG = '/kaniko/.docker/'
            }
            steps {
                withCredentials([string(credentialsId: 'docker-config-base64', variable: 'DOCKER_CONFIG_BASE64')]) {
                    sh '''
                    mkdir -p /kaniko/.docker
                    echo $DOCKER_CONFIG_BASE64 > /kaniko/.docker/config.json
                    /kaniko/executor --dockerfile `pwd`/Dockerfile_app \
                                     --context `pwd` \
                                     --destination=petrobubka/my_gogs_image_swarm:latest \
                                     --cache=true
                    '''
                }
            }
        }
        stage('Deploy App to Docker Swarm') {
            agent any
            steps {
                script {
                    sh 'docker stack deploy -c docker-compose-gogs.yml gogs-stack'
                }
            }
        }
    }
}
