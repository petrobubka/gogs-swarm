pipeline {
    agent any // Define no global agent; use specific agents per stage
    stages {
        stage ('Build image'){
            steps{
                script{
                    withDockerServer([uri:'tcp://192.168.100.30:2376']) {
                        sh "docker ps"
                    }
                }
            }
        }


    }
}
