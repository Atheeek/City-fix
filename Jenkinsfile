pipeline {
  agent any

  stages {
    stage('Clone Repository') {
      steps {
        git url: 'https://github.com/Atheeek/City-fix.git', branch: 'main'
      }
    }

    stage('Build Docker Images') {
      steps {
        sh 'docker-compose build'
      }
    }

    stage('Start Containers') {
      steps {
        sh 'docker-compose up -d'
      }
    }
  }
}
