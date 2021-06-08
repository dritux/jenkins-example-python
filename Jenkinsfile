def label = "mypod-${UUID.randomUUID().toString()}"

podTemplate(label: label, containers: [
    containerTemplate(name: 'python', image: 'python:3.7-alpine', ttyEnabled: true, command: 'cat'),
]) {
    node(label) {
        stages { 
            stage('Checkout') {
              steps {
                checkout scm
              }
            }
            stage('Setup') {
              steps {
                script {
                  sh """
                  pip install -r requirements.txt
                  """
                }
              }
            }
            stage('Test a Python project') {
                steps{
                    sh 'python test.py'
                }
            }
        }
    }
}