pipeline {
  agent any

  environment {
    BACKEND_REPO = 'https://github.com/subash-thiruppathi/approval-flow-nodejs-backend.git'
    FRONTEND_REPO = 'https://github.com/subash-thiruppathi/expense-tracker-front-end.git'
    BACKEND_DIR = 'approval-flow-nodejs-backend'
    FRONTEND_DIR = 'expense-tracker-front-end'
    GIT_CREDENTIALS_ID = 'd335fd60-d7d0-4313-afb8-80b497004463' // your existing Jenkins GitHub credential ID
  }

  stages {
    stage('Clone Backend') {
      steps {
        dir("${BACKEND_DIR}") {
          git branch: 'master', credentialsId: "${GIT_CREDENTIALS_ID}", url: "${BACKEND_REPO}"
        }
      }
    }

    stage('Clone Frontend') {
      steps {
        dir("${FRONTEND_DIR}") {
          git branch: 'main', credentialsId: "${GIT_CREDENTIALS_ID}", url: "${FRONTEND_REPO}"
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
