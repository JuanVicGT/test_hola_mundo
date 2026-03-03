pipeline {
  agent any

  options {
    timestamps()
    disableConcurrentBuilds()
  }

  environment {
    REPO_URL = 'https://github.com/JuanVicGT/test_hola_mundo.git'
    BRANCH   = 'main'
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: "${BRANCH}", url: "${REPO_URL}"
        sh 'ls -la'
      }
    }

    stage('Verify file') {
      steps {
        sh '''
          test -f index.html
          echo "OK: index.html existe"
        '''
      }
    }

    stage('HTML lint (docker)') {
      steps {
        sh '''
          echo "Validando HTML con htmlhint dentro de un contenedor Node..."
          docker run --rm -v "$PWD:/work" -w /work node:20-alpine sh -lc "
            npm -s i -g htmlhint >/dev/null 2>&1
            htmlhint index.html
          "
        '''
      }
    }

    stage('Archive artifact') {
      steps {
        archiveArtifacts artifacts: 'index.html', fingerprint: true
      }
    }
  }

  post {
    always {
      echo "Resultado: ${currentBuild.currentResult}"
    }
  }
}