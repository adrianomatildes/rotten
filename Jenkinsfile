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
          def imageName = "amatildes/rotten:${env.BUILD_ID}"
          def dockerapp = docker.build(imageName, "-f src/Dockerfile .")
          env.IMAGE_NAME = imageName // Defina uma variável de ambiente com o nome da imagem
        }
      }
    }

    stage('Push Image') {
      steps {
        script {
          // Push da imagem Docker para o registro DockerHub
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            docker.image(env.IMAGE_NAME).push('latest') // Use a variável de ambiente para referenciar a imagem
            docker.image(env.IMAGE_NAME).push("${env.BUILD_ID}") // Use a variável de ambiente para referenciar a imagem
          }
        }
      }
    }

    stage('Deploy Kubernetes') {
      agent {
        kubernetes {
          cloud 'kubernetes'
          label 'kubepods' // Substitua pela etiqueta correta do seu agente Kubernetes
          containerTemplate {
            name 'jnlp'
            resourceRequestCpu '0.5' // CPU solicitada (0.5 núcleos)
            resourceRequestMemory '1Gi' // Memória solicitada (1 gigabyte)
            resourceLimitCpu '1' // Limite de CPU (1 núcleo)
            resourceLimitMemory '2Gi' // Limite de memória (2 gigabytes)
          }
        }
      }
      environment {
        tag_version = "${env.BUILD_ID}"
      }
      steps {
        script {
          sh 'sed -i "s/{{tag}}/$tag_version/g" ./k8s/api/deployment.yaml'
          sh 'cat ./k8s/api/deployment.yaml'
          kubernetesDeploy(configs: '**/k8s/**', kubeconfigId: 'kubeconfig')
        }
      }
    }
  }
}