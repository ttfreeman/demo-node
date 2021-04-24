pipeline {
    agent any

    stages {
        stage('Initialize') {
            agent {
                kubernetes {
                    // label "${env.BRANCH_NAME}-node"
                    containerTemplate {
                        name 'node'
                        image 'node:8-jessie'
                        ttyEnabled true
                        command 'cat'
                    }
                }
            }
            steps {
                container('node') {
                    sh 'npm install -g serverless'
                }
                echo "${env.JOB_NAME}"
            }
        }
        
    }
}
