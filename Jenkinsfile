pipeline {
    agent none // Use none because we will specify the environment for each stage
    stages {
        
        stage('Kaniko Build & Push Image') {
            agent {
                docker {
                    image 'gcr.io/kaniko-project/executor:debug'
                    args '-v /var/run/docker.sock:/var/run/docker.sock --entrypoint='
                }
            }
            steps {
                withDockerRegistry(credentialsId: 'my-docker-credentials', url: 'https://index.docker.io/v1/') {
                sh '''
                /kaniko/executor --dockerfile `pwd`/Dockerfile_app \
                                 --context `pwd` \
                                 --destination=petrobubka/my_gogs_image:${BUILD_NUMBER}
                '''
                 }

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
