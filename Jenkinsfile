pipeline {
  agent any

  environment {
    MONGO_URI = credentials('mongo-uri')
    JWT_SECRET = credentials('jwt-secret')
  }

  stages {
    stage('Clone Repository') {
      steps {
        git url: 'https://github.com/Atheeek/City-fix.git', branch: 'main'
      }
    }

    stage('Generate .env File') {
      steps {
        writeFile file: '.env', text: """\
MONGO_URI=${MONGO_URI}
JWT_SECRET=${JWT_SECRET}
PORT=5000
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
