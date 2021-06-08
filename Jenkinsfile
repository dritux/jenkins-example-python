def label = "mypod-${UUID.randomUUID().toString()}"

podTemplate(label: label, containers: [
    containerTemplate(name: 'python', image: 'python:3.7-alpine', ttyEnabled: true, command: 'cat'),
]) {
    node(label) {
        stage('Get a Python project') {
            git 'https://github.com/dritux/terraform-kubernetes-jenkins.git'
            container('python') {
                            stage('Checkout') {
              steps {
                checkout scm
              }
            }
            }
        }
    }
}