pipeline {
  environment {
    registry = "mjaballi/odoo-test"
    registryCredential = 'DOCKER_HUB_CRED'
    dockerImage = ''
  }
  agent any
  tools {nodejs "node" }
  stages {
    stage('Cloning Git') {
      steps {
         git branch: '10.0', credentialsId: '571e0fd7-2841-4c22-ada6-92cf2febc352', url: 'https://github.com/maherjaballi/odoo.git'
      }
    }
    stage('Build') {
       steps {
         sh 'npm install'
       }
    }
    stage('Test') {
      steps {
        sh 'npm test'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
     
  }
}
