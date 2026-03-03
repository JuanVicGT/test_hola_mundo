pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/JuanVicGT/test_hola_mundo.git'
            }
        }

        stage('List files') {
            steps {
                sh 'ls -la'
            }
        }

        stage('Verify HTML') {
            steps {
                sh 'cat index.html'
            }
        }

        stage('Check Docker') {
            steps {
                sh 'docker version'
            }
        }

        stage('Archive artifact') {
            steps {
                archiveArtifacts artifacts: 'index.html'
            }
        }

    }
}