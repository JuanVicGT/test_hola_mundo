pipeline {
  agent any

  options {
    timestamps()
    disableConcurrentBuilds()
  }

  stages {

    stage('BajarCodigo') {
      steps {
        // Usa el repo/branch definidos en "Pipeline script from SCM"
        checkout scm
      }
    }

    stage('List files') {
      steps {
        sh 'pwd'
        sh 'ls -la'
      }
    }

    stage('Verify index.html') {
      steps {
        sh '''
          set -e
          test -f index.html
          echo "OK: index.html existe"
          echo "Preview:"
          head -n 30 index.html
        '''
      }
    }

    stage('Dumie test') {
      steps {
        sh '''
          set -e
          test -f index.html
          echo "OK: index.html existe"
          echo "Preview:"
          head -n 30 index.html
        '''
      }
    }

    stage('Archive artifact') {
      steps {
        archiveArtifacts artifacts: 'index.html', fingerprint: true, allowEmptyArchive: false
      }
    }
  }

  post {
    always {
      echo "Resultado final: ${currentBuild.currentResult}"
    }
  }
}