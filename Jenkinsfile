pipeline {
    agent none // No global agent, define per stage
    stages {
        stage('Build') {
            agent { label 'my-docker-agent' } // This label matches the Docker template label
            steps {
                sh 'echo "Hello from Docker agent"'
            }
        }
    }
}
