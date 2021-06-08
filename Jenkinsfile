pipeline {
    agent { docker { image 'python:3.7.2' } }
    stages {
        stage('build') {
            steps {
                sh 'python --version'
            }
        }
        stage('STAGE 00'){
            steps{
                echo "Pipeline Usando Jenkinsfile"
            }
        }
        stage('STAGE 01'){
            steps{
                echo "Pipeline Usando Jenkinsfile"
            }
        }
        stage('build'){
            steps{
                sh 'pip install -r requirements.txt'
            }
        }
        stage('test'){
            steps{
                sh 'python test.py'
            }
        }
    }
}