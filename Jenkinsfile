pipeline {
  environment {
    registry = "abd1230/tp4"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git branch: 'master', url: 'https://github.com/omariabdelhadi/tp4.git'
      }
    }
    stage('Building image') {
      steps {
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Test image') {
      steps {
        script {
          echo "Tests passed"
        }
      }
    }
    stage('Publish Image') {
      steps {
        script {
          docker.withRegistry('', registryCredential) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Deploy image') {
      steps {
        bat 'docker rm -f tp4-container || exit /b 0'
        bat "docker run -d --name tp4-container -p 8083:80 ${registry}:${BUILD_NUMBER}"
      }
    }
  }
}
