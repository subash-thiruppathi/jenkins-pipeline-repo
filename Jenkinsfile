pipeline {
  agent any

  environment {
    BACKEND_REPO = 'https://github.com/subash-thiruppathi/approval-flow-nodejs-backend'
    FRONTEND_REPO = 'https://github.com/subash-thiruppathi/expense-tracker-front-end'
    BACKEND_DIR = 'approval-flow-nodejs-backend'
    FRONTEND_DIR = 'expense-tracker-front-end'
  }

  stages {
    stage('Clone Backend') {
      steps {
        dir("${BACKEND_DIR}") {
          git url: "${BACKEND_REPO}", changelog: false, poll: false
        }
      }
    }

    stage('Clone Frontend') {
      steps {
        dir("${FRONTEND_DIR}") {
          git url: "${FRONTEND_REPO}", changelog: false, poll: false
        }
      }
    }

    stage('Build Backend') {
      steps {
        dir("${BACKEND_DIR}") {
          sh 'docker build -t expense-backend .'
        }
      }
    }

    stage('Build Frontend') {
      steps {
        dir("${FRONTEND_DIR}") {
          sh 'docker build -t expense-frontend .'
        }
      }
    }

    stage('Deploy') {
      steps {
        sh 'docker compose -f docker-compose.yml down || true'
        sh 'docker compose -f docker-compose.yml up -d --build'
      }
    }
  }

  post {
    always {
      cleanWs()
    }
    success {
      echo 'Pipeline completed successfully!'
    }
    failure {
      echo 'Pipeline failed!'
    }
  }
}
