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
            image: 15.14.0-alpine3.10
            command:
            - cat
            tty: true
          - name: busybox
            image: busybox
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
          sh 'node -version'
          sh "echo Workspace dir is ${pwd()}"
          sh "echo Workspace content is ${ls()}"
        }
        container('busybox') {
          sh '/bin/busybox'
        }
      }
    }
  }
}
