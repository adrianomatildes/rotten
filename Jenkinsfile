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
        sh 'docker build -t amatildes/rotten-potatoes:${env.BUILD_ID} -f src/Dockerfile .'
      }
    }

    stage('Push Image') {
      steps {
        sh 'docker push amatildes/rotten-potatoes:${env.BUILD_ID}'
      }
    }
  }
}