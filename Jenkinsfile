pipeline {
  agent any

  options {
    timestamps()
    disableConcurrentBuilds()
  }

  stages {

    stage('Bajar código') {
      steps {
        // Usa el repo/branch definidos en "Pipeline script from SCM"
        checkout scm
      }
    }

    stage('Listar archivos') {
      steps {
        sh 'pwd'
        sh 'ls -la'
      }
    }

    stage('Verificar index.html') {
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

    stage('Validar title HTML') {
      steps {
        sh '''
          set -e

          if grep -q "<title>hola-mundo</title>" index.html; then
            echo "OK: title correcto"
          else
            echo "ERROR: title incorrecto"
            exit 1
          fi
        '''
      }
    }

    stage('Verificar Docker') {
      steps {
        // Para verificar que Jenkins tiene acceso a Docker
        sh 'docker --version'
      }
    }

    stage('Levantar Imagen Docker') {
      steps {
        sh 'docker build -t hola-jenkins-image .'
      }
    }

    stage('Quitar contenedor viejo') {
      steps {
        sh 'docker rm -f hola-jenkins-container || true'
      }
    }

    stage('Levantar contenedor') {
      steps {
        sh 'docker run -d -p 8081:80 --name hola-jenkins-container hola-jenkins-image'
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