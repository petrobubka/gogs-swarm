pipeline {
    agent any // Define no global agent; use specific agents per stage
    stages {
        stage('Setup Environment') {
            steps {
                sh '''
                   docker ps
                '''
            }
        }


    }
}
