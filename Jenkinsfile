pipeline {
    agent any

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
    }
}
