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
            volumeMounts:
            - mountPath: /var/run/docker.sock
              name: docker-sock
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
            volumeMounts:
            - mountPath: /var/run/docker.sock
              name: docker-sock
          - name: docker
            image: docker:dind
            command:
            - cat
            tty: true
            securityContext:
              privileged: true
            volumeMounts:
            - mountPath: /var/run/docker.sock
              name: docker-sock
          - name: kaniko
            image: gcr.io/kaniko-project/executor:debug-539ddefcae3fd6b411a95982a830d987f4214251
            imagePullPolicy: Always
            command:
            - /busybox/cat
            tty: true
            volumeMounts:
              - name: jenkins-docker-cfg
                mountPath: /kaniko/.docker
          volumes:
            - name: docker-sock
              emptyDir: {}
            - name: jenkins-docker-cfg
              projected:
                sources:
                - secret:
                    name: regcred
                    items:
                      - key: .dockerconfigjson
                        path: config.json
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
    stage('Sonar Test') {
      steps {
        container('sonar') {
          sh '''
          pwd
          ls -la
          sonar-scanner
          '''
        }
      }
    }
    stage('Kaniko Build') {
      steps {
      container('kaniko') {
        sh '/kaniko/executor -f `pwd`/Dockerfile -c `pwd` --insecure --skip-tls-verify --cache=true --destination=mydockerregistry:5000/myorg/myimage'
      }
      }
    }
    stage('Docker Build') {
      steps {
        container('docker') {
          sh '''
          docker --version
          ls -la
          whoami
          docker build -t demo-node:$BUILD_NUMBER .
          '''
        }
      }
    }
  }
}
