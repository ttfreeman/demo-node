pipeline {
    agent {
      label 'docker' 
    }

    stages {
        agent { docker { 
            label 'docker'
            image 'node:14-alpine' 
        } }
        stage('build') {
            steps {
                sh 'npm --version'
            }
        }
    }
}
