pipeline {
    agent any

    stages {
        stage('Build Image') {
            steps {
                script {
                    docker.build("my-app:${env.BUILD_ID}")
                }
            }
        }
    }
}
