pipeline {
    agent {
      label 'docker' 
    }

    stages {

        stage('build') {
            agent { 
                docker { 
                  label 'docker'
                  image 'node:14-alpine' 
              } 
            }
            steps {
                sh 'npm --version'
            }
        }
    }
}
