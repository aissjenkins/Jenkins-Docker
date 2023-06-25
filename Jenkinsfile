pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('a3bce06f-f7f2-482e-b610-8b793083b8f2')
  }
  stages {
    stage('Build') {
      steps {
        node('maître') {
          sh 'docker build -t aissjenkins/jenkins-docker-hub .'
        }
      }
    }
    stage('Login') {
      steps {
        node('maître') {
          withCredentials([usernamePassword(credentialsId: 'DOCKERHUB_CREDENTIALS', usernameVariable: 'DOCKERHUB_CREDENTIALS_USR', passwordVariable: 'DOCKERHUB_CREDENTIALS_PSW')]) {
            sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
          }
        }
      }
    }
    stage('Push') {
      steps {
        node('maître') {
          sh 'docker push aissjenkins/jenkins-docker-hub'
        }
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}

