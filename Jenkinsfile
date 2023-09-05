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
          def dockerapp = docker.build("amatildes/rotten-potatoes:${env.BUILD_ID}", "--build-arg BUILD_DATE=`date -u +'%Y-%m-%dT%H:%M:%SZ'` --build-arg VCS_REF=`git rev-parse --short HEAD` -f src/Dockerfile . --no-cache")
        }
      }
    }

    stage('Push Image') {
      steps {
        script {
          // Push da imagem Docker para o registro DockerHub
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            dockerapp.push('latest')
            dockerapp.push("${env.BUILD_ID}")
          }
        }
      }
    }
  }
}