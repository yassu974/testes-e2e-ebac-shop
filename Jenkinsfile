pipeline {
  agent any

  options {
    timestamps()
  }

  environment {
    NO_COLOR = '1'
    TERM = 'xterm'
  }

  stages {
    stage('Setup') {
      steps {
        sh 'npm install'
        sh 'npx browserslist@latest --update-db || true'
      }
    }

    stage('Test') {
      steps {
        sh 'npm run cy:run'
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'cypress/screenshots/**/*, cypress/videos/**/*', allowEmptyArchive: true

      publishHTML(target: [
        allowMissing: true,
        alwaysLinkToLastBuild: true,
        keepAll: true,
        reportDir: 'mochawesome-report',
        reportFiles: 'mochawesome.html',
        reportName: 'EBAC Report'
      ])
    }
    success {
      echo '✅ Pipeline finalizado com sucesso.'
    }
    failure {
      echo '❌ Pipeline falhou — veja o relatório Mochawesome e os artefatos.'
    }
  }
}