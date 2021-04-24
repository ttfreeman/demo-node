// //Added shared Library
// library identifier: 'cicdjenkins@master', retriever: modernSCM(
//     [$class: 'GitSCMSource',
//     remote: 'github.com/jenkinpiple/',
//     credentialsId: 'svc.devops-ut']
// )

//Defines the build CI pipeline for app
pipeline {
    // agent { 
    //     kubernetes {
    //         label 'app'
    //         defaultContainer 'jnlp'
    //         yamlFile 'Jenkins.yaml' 
    //     }
    // }
    
    environment {
            DOCKER_REGISTRY= 'docker.io'
            APP_NAME = 'demo-node'
            DOCKER_REGISTRY_CRED_ID= "ttfreeman"
            DOCKER_REPOSITORY= 'docker-local'
    }
   
    stages {
        // //Purpouse : Notify slack about JOB started
        // stage('General') {
        //     steps {
        //         notify('STARTED')
        //         githubstatus('STARTED')
        //         echo sh(script: 'env|sort', returnStdout: true)
        //             script {
        //                 USER = wrap([$class: 'BuildUser']) {
        //                 return env.BUILD_USER
        //             }
        //             GIT_REVISION = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
        //         }    
        //     }
        // }

        // //Docker Build
        // stage('Build') {
        //     steps {
        //         container('docker') {
        //             script{
        //                 def git_branch = "${GIT_BRANCH}"
        //                 git_branch = git_branch.replace("/", "_")
        //                 DOCKER_BUILD_NUMBER = BUILD_NUMBER+"-${git_branch}"
        //                 docker.build("${DOCKER_REGISTRY}/${DOCKER_REPOSITORY}/${APP_NAME}"+":"+DOCKER_BUILD_NUMBER)  
        //             }
        //         }
        //     }
        // }
        // stage('Test') {
        //     steps {
        //         container('nodejs') {
        //             script {
        //                 output = sh(returnStdout: true, script: 'npm install --no-cache && npm test' ).trim()
        //                 echo output
        //             }
        //         }
        //     }
        // }

        stage("SonarQube Analysis") {
            steps{
                //Executing SonarQube Analysis inside build container
                container("sonar"){
                    script {
                            sh "sed -i 's/sonar.projectVersion=build-number/sonar.projectVersion=${BUILD_NUMBER}/g' sonar-project.properties"
                            sh "sed -i 's@sonar.branch.name=branch_name@sonar.branch.name=$BRANCH_NAME@g' sonar-project.properties"
                            withSonarQubeEnv('SonarQube') {
                                echo "===========Performing Sonar Scan============"
                                def sonarqubeScannerHome = tool 'SonarQube Scanner 3.3.0.1492'
                                sh "${sonarqubeScannerHome}/bin/sonar-scanner"
                            }
                    }
                }
            }
        }

        // //Quality Gate 
        // stage("Quality Gate") {
        //     steps {
        //         script {
        //             timeout(time: 1, unit: 'HOURS') {
        //             waitForQualityGate abortPipeline: true
        //             }
        //         }

        //     }
        // }
        // //Docker Push   
        // stage('Artifactory Push'){
        //     steps {
        //         container('docker') {
        //             script {
        //                 dockerpush("${DOCKER_REPOSITORY}/${APP_NAME}","${DOCKER_BUILD_NUMBER}","${DOCKER_REPOSITORY}")
        //             }
        //         }
        //     }
        // }
    }
}