pipeline {
  agent {
    kubernetes {
      yaml """\
        apiVersion: v1
        kind: Pod
        metadata:
          labels:
            some-label: some-label-value
        securityContext: 
          allowPrivilegeEscalation: false
          runAsUser: 0
        spec:
          containers:
          - name: nodejs
            image: node:15.14.0-alpine3.10
            command:
            - cat
            tty: true
          - name: sonar
            image: sonarsource/sonar-scanner-cli
            command:
            - cat
            env:
            - name: SONAR_HOST_URL
              value: https://sonarqube-dbbsbxarmnsp001.apps.mj0pbxvw.westus2.aroapp.io
            - name: SONAR_LOGIN
              value: 9ff1a72d0d927c4b0b57a67f51321f8490c3115f
            tty: true
          - name: oc
            image: ebits/openshift-client
            imagePullPolicy: Always
            command:
            - cat
            tty: true
        """.stripIndent()
    }
  }
  stages {
    stage('Run nodejs') {
      steps {
        container('nodejs') {
          sh '''
          node --version
          pwd
          ls -la
          '''
        }
      }
    }
    // stage('Sonar Test') {
    //   steps {
    //     container('sonar') {
    //       sh '''
    //       pwd
    //       ls -la
    //       sonar-scanner
    //       '''
    //     }
    //   }
    // }
    stage('Run oc') {
      steps {
        container('oc') {
          sh '''
          oc version
          ls -la
          whoami
          oc whoami
          pwd
          oc login --token=sha256~veEfJt4PNsobIHhG_RExfWUbROXAihhNnaddNa0-j-E --server=https://api.mj0pbxvw.westus2.aroapp.io:6443
          oc delete bc,dc,deployment,svc,route -l app=demo-node 
          oc new-build . --name=demo-node --strategy=docker
          oc start-build demo-node --wait=true

          oc new-app demo-node:latest
          oc expose svc/demo-node

          '''
        }
      }
    }
  }
}
