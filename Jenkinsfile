pipeline {
  agent any

  stages {
    stage('Checkout Source') {
      steps {
        script {
          // Checkout do repositório Git
          git url: 'https://github.com/adrianomatildes/rotten.git', branch: 'master'
        }
      }
    }

    stage('Build image') {
      steps {
        script {
          // Construção da imagem Docker
          def dockerapp = docker.build("amatildes/rotten-potatoes:${env.BUILD_ID}", "-f src/Dockerfile .")
        }
      }
    }

    stage('Push Image') {
      steps {
        script {
          // Push da imagem Docker para o registro DockerHub
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            def dockerapp.push('latest')
            def dockerapp.push("${env.BUILD_ID}")
          }
        }
      }
    }
  }
}