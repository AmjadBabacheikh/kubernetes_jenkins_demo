def d = [
  'node.version':'16-alpine3.12'
]

def props = [:]

podTemplate {
  node(POD_LABEL) {
    checkout scm
    props = readProperties(defaults: d, file: 'version.properties')
  }
}
pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: node
            image: node:${props["node.version"]}
            command:
            - cat
            tty: true
        '''
    }
  }
  stages {
    stage('Run maven') {
      steps {
        container('node') {
          sh 'npm version'
        }
      }
    }
  }
}
