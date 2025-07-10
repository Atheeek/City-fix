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

    stage('Build Docker Image') {
      steps {
        bat 'docker build -t atheekrhmn/cityfix-backend ./backend'
      }
    }

    stage('Push to Docker Hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          bat '''
            echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
            docker push atheekrhmn/cityfix-backend
          '''
        }
      }
    }

    stage('Deploy to EC2 via SSH') {
      steps {
        sshagent (credentials: ['ec2-ssh-key']) {
          bat '''
            ssh -o StrictHostKeyChecking=no ec2-user@16.170.212.110 ^
              "docker pull atheekrhmn/cityfix-backend && ^
              docker stop cityfix-backend || echo Not running && ^
              docker rm cityfix-backend || echo Not running && ^
              docker run -d -p 5000:5000 -e MONGO_URI=%MONGO_URI% -e JWT_SECRET=%JWT_SECRET% --name cityfix-backend atheekrhmn/cityfix-backend"
          '''
        }
      }
    }
  }
}
