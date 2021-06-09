def label = "mypod-${UUID.randomUUID().toString()}"

podTemplate(
    label: label, 
    containers: [
        containerTemplate(name: 'python', image: 'python:3.7-alpine', ttyEnabled: true, command: 'cat')
    ]) {
    node(label) {
        stage('Get a Python project') {
            git 'https://github.com/dritux/terraform-kubernetes-jenkins.git'
            container('python') {
                stage('Setup environment') {
                    checkout scm
                    container('python') {
                        sh """
                            python --version
                            python -m pip install -r requirements.txt
                        """
                    }
                }
                stage('Setup Tests') {
                    checkout scm
                    container('python') {
                        sh "python test.py"
                    }
                }

            }
        }
        stage('Build image') {
            app = docker.build("us.gcr.io/fs-phone-diagnostics")
        }
        stage('Push image') {
          docker.withRegistry('https://us.gcr.io', 'gcr:fs-phone-diagnostics') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
          }
        }
    }
}