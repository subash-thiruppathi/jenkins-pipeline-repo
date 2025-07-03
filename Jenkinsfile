pipeline {
  agent any

  environment {
    BACKEND_REPO = 'https://github.com/subash-thiruppathi/approval-flow-nodejs-backend.git'
    FRONTEND_REPO = 'https://github.com/subash-thiruppathi/expense-tracker-front-end.git'
    BACKEND_DIR = 'expense-approval-api'
    FRONTEND_DIR = 'expense-approval-front-end'
  }

  stages {
    stage('Clone Backend') {
      steps {
        dir("${BACKEND_DIR}") {
          git url: "${BACKEND_REPO}", branch: 'master', credentialsId: 'github-credentials-id'
        }
      }
    }

    stage('Clone Frontend') {
      steps {
        dir("${FRONTEND_DIR}") {
          git url: "${FRONTEND_REPO}", branch: 'main', credentialsId: 'github-credentials-id'
        }
      }
    }

    stage('Create .env') {
      steps {
        withCredentials([
          string(credentialsId: 'DB_HOST', variable: 'DB_HOST'),
          string(credentialsId: 'DB_PORT', variable: 'DB_PORT'),
          string(credentialsId: 'DB_NAME', variable: 'DB_NAME'),
          string(credentialsId: 'DB_USER', variable: 'DB_USER'),
          string(credentialsId: 'DB_PASS', variable: 'DB_PASS')
        ]) {
          dir("${BACKEND_DIR}") {
            writeFile file: '.env', text: """
    DB_HOST=${DB_HOST}
    DB_PORT=${DB_PORT}
    DB_NAME=${DB_NAME}
    DB_USER=${DB_USER}
    DB_PASS=${DB_PASS}
            """.stripIndent()
          }
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
      // cleanWs()
    }
    success {
      echo 'Pipeline completed successfully!'
    }
    failure {
      echo 'Pipeline failed!'
    }
  }
}
