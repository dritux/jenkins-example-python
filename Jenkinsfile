def label = "mypod-${UUID.randomUUID().toString()}"

podTemplate(
    label: label, 
    containers: [
        containerTemplate(name: 'python', image: 'python:3.7-alpine', ttyEnabled: true, command: 'cat')
        containerTemplate(name: 'docker',
            command: '/bin/cat -',
            image: 'docker:18.06.1-ce',
            resourceLimitCpu: '1000m',
            resourceLimitMemory: '1000Mi',
            ttyEnabled: true),
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
            container('docker') {
                docker.withRegistry('https://us.gcr.io', 'gcr:fs-phone-diagnostics') {
                    app = docker.build("us.gcr.io/fs-phone-diagnostics", "-f Dockerfile .")
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                }
            }
        }
    }
}