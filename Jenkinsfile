pipeline {
    agent none // Use none because we will specify the environment for each stage
    stages {
        
        
        stage('Deploy App to Docker Swarm') {
            agent any // Run on any available agent
            steps {
                script {
                    sh 'docker ps'
                }
            }
        }
    }
}
