pipeline {
  agent any
  
  stages {
    stage('checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }
    
    stage('Run Tests') {
      steps {
        sh 'npm test'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build --platform linux/amd64 -t jmin99/express-hello-world:latest .'
      }

    stage('Push to Docker Hub') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhub-login',
          usernameVariable: 'DOCKER_USERNAME',
          passwordVariable: 'DOCKER_PASSWORD', 
        )]) {
          sh '''
          echo "$DOCKER_TOKEN" | docker login -u "$DOCKER_USERNAME" --password-stdin
          docker push "$DOCKER_USERNAME"/express-hello-word:latest
          '''
        }
      }
    }
   
    stage('Deploy via Docker Compse') {
      steps {
        sh '''
        cd /var/lib/jenkins/backend
        docker compose pull
        docker compose up -d
        '''
      }
    }
  }
}
