pipeline {
    agent any

    stages {
        stage('Initialize') {
            agent {
                kubernetes {
                    // label "${env.BRANCH_NAME}-node"
                    containerTemplate {
                        name 'node'
                        image 'node:15.14.0-alpine3.10'
                        ttyEnabled true
                        command 'cat'
                    }
                }
            }
            steps {
                container('node') {
                    sh "echo ${whoami}"
                    sh "echo ${pwd}"
                    sh "echo ${ls -la}"
                    // sh 'npm install'
                }
                echo "${env.JOB_NAME}"
            }
        }
        
    }
}
