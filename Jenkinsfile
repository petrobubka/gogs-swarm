pipeline {
      agent {
        swarm {
          image 'alpine:3.15'
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
