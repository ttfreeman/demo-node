pipeline {
  agent {
    kubernetes {
      yaml """\
        apiVersion: v1
        kind: Pod
        metadata:
          labels:
            some-label: some-label-value
        spec:
          containers:
          - name: nodejs
            image: node:15.14.0-alpine3.10
            command:
            - cat
            tty: true
          - name: docker
            image: docker:dind
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
        container('docker') {
          sh '''
          docker --version
          pwd
          ls -la
          docker build .
          '''
        }
      }
    }
  }
}
