pipeline {
  agent any
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub-')  
  }
  stages {
    stage('Checkout Source') {
      steps {
        git 'https://github.com/cubensquare/myproject.git'
      }
    }

    stage('Build image') {
      steps{
        sh 'docker build -t "cubensquare/python:$BUILD_NUMBER"  "$WORKSPACE"/'
        }
      }
  
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }

    stage('Push Image') {
      steps{
         sh 'docker push cubensquare/python:$BUILD_NUMBER'
      }
    }

    post {
     always {
      sh 'docker logout'
      }
    }
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "frontend.yaml", kubeconfigId: "kube")
        }
      }
    }

  }
 }
