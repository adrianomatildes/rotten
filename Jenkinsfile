pipeline {
  agent any

  stages {
    stage('Checkout Source') {
      steps {
        git url: 'https://github.com/adrianomatildes/rotten.git', branch: 'master'
      }
    }

    stage('Build image') {
      steps {
        script {
          dockerapp = docker.build("amatildes/rotten:${env.BUILD_ID}", '-f ./src/Dockerfile .')
        }
      }
    }

    stage('Push Image') {
      steps {
        script {
          docker.withRegistry('https:registry.hub.docker.com', 'dockerhub') {
          dockerapp.push('latest')
          dockerapp.push("${env.BUILD_ID}")
          }
        }
      }
    }
  }
}