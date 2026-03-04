pipeline {
  agent any

  options {
    timestamps()
    disableConcurrentBuilds()
  }

  stages {

    stage('Checkout') {
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

    stage('HTML lint') {
      steps {
        script {
          // Si no hay docker en el agente, no tumbes el pipeline:
          // intenta y si falla, lo marca como UNSTABLE y continúa.
          try {
            sh '''
              set -e
              echo "Validando HTML con htmlhint usando Node en Docker..."
              docker version >/dev/null 2>&1
              docker run --rm \
                -v "$PWD:/work" \
                -w /work \
                node:20-alpine sh -lc "
                  npm -s i -g htmlhint >/dev/null 2>&1
                  htmlhint index.html
                "
            '''
          } catch (err) {
            echo "WARN: No se pudo ejecutar el lint (docker no disponible o error de htmlhint). Continuando..."
            currentBuild.result = 'UNSTABLE'
          }
        }
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