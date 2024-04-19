pipeline {
    agent none // Use none because we will specify the environment for each stage
    stages {
        stage('Install dependencies') {
            agent {
                docker {
                    image 'alpine:3.15'
                    args '-u root'
                }
            }
            steps {
                sh '''
                echo -e "https://alpine.global.ssl.fastly.net/alpine/v3.18/community" > /etc/apk/repositories
                echo -e "https://alpine.global.ssl.fastly.net/alpine/v3.18/main" >> /etc/apk/repositories
                apk update
                apk add --no-cache binutils go postgresql-client git openssh
                go build -o gogs -buildvcs=false
                go test -v -cover ./...
                '''
            }
        }
                
        
        stage('Kaniko Build & Push Image') {
            agent {
                docker {
                    image 'gcr.io/kaniko-project/executor:debug'
                    args '-u root'
                }
            }
            steps {
                sh '''
                /kaniko/executor --dockerfile `pwd`/Dockerfile_app \
                                 --context `pwd` \
                                 --destination=petrobubka/my_gogs_image:${BUILD_NUMBER}
                '''
            }
        }
        
        stage('Deploy App to Docker Swarm') {
            agent any // Run on any available agent
            steps {
                script {
                    sh 'docker stack deploy -c docker-compose-gogs.yml gogs'
                }
            }
        }
    }
}
