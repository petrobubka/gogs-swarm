pipeline {
    agent none // Use 'none' because we specify the environment for each stage

    stages {
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
                                     --destination=petrobubka/my_gogs_image_nomad:2 \
                                     --cache=true
                    '''
                }
            }
        }
    }
}
