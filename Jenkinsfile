pipeline {
  agent any

  environment {
    MONGO_URI = credentials('mongo-uri')
    JWT_SECRET = credentials('jwt-secret')
  }

  stages {
    stage('Clone Repo') {
      steps {
        git url: 'https://github.com/Atheeek/City-fix.git', branch: 'main'
      }
    }

    stage('Generate .env files') {
      steps {
        writeFile file: 'backend/.env', text: """\
MONGO_URI=${MONGO_URI}
JWT_SECRET=${JWT_SECRET}
PORT=5000
"""

        writeFile file: 'frontend/.env', text: """\
VITE_API_BASE_URL=http://localhost:5000
"""
      }
    }

    stage('Build Docker Images') {
      steps {
        bat 'docker-compose build'
      }
    }

    stage('Start Containers') {
      steps {
        bat 'docker-compose up -d'
      }
    }
  }
}
