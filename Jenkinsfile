pipeline {
  agent any
  stages {
    stage('Clone repository') {
      steps {
        checkout scm
      }
    }

    stage('Build image') {
      steps {
        script {
          def app = docker.build("slavcho/kiii-jenkins")
        }

      }
    }

    stage('Push image') {
      steps {
        script {
          docker.withRegistry(env.DOCKER_REGISTRY, env.DOCKER_CREDENTIALS_ID) {
            app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
            app.push("${env.BRANCH_NAME}-latest")
            // signal the orchestrator that there is a new version
          }
        }

      }
    }

  }
  environment {
    DOCKER_REGISTRY = 'https://registry.hub.docker.com'
    DOCKER_CREDENTIALS_ID = 'cc379a89-6a68-4dfd-b2a7-2927e6b6aa62'
  }
}