def label = "mypod-${UUID.randomUUID().toString()}"

podTemplate(
    label: label, 
    containers: [
        containerTemplate(name: 'python', image: 'python:3.7-alpine', ttyEnabled: true, command: 'cat'),
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

                stage("docker build"){
                    Img = docker.build(
                        "fs-phone-diagnostics/example:${env.BUILD_NUMBER}",
                        "-f Dockerfile ."
                    )
                }
                stage("docker push") {
                    docker.withRegistry('https://gcr.io', "gcr:fs-phone-diagnostics") {
                        Img.push(${env.BUILD_NUMBER})
                    }
                }
            }
        }
    }
}