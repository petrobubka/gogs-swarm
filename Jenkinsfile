pipeline {
      agent {
        dockerContainer {
          image 'benhall/dind-jenkins-agent:v2'
        }
      }
    
    stages {
        stage('Echo Message') {
            steps {
                echo 'Hello, world!'
            }
        }
    }
}
