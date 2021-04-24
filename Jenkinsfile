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
            image: docker:latest
            command:
            - cat
            tty: true
            volumeMounts:
            - mountPath: /var/run/docker.sock
              name: docker-sock
          volumes:
            - name: docker-sock
              hostPath:
              path: /var/run/docker.sock

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
          docker info
          pwd
          ls -la
          docker build -t demo-node:$BUILD_NUMBER .
          '''
        }
      }
    }
  }
}
