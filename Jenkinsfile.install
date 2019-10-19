pipeline {
  environment {
    registry = "cnadella/docker-test"
    registryCredential = 'DockerHub'
    dockerImage = ''
  }
  agent any
  stages {
    stage ("Waiting to Make sure Helm Repo Can be pulled in") {
      echo 'Waiting 5 minutes for deployment to complete prior starting smoke testing'
      sleep 300 // seconds
    }
    stage('Cloning Git') {
      steps {
        checkout scm
      }
    }
    stage('Update Local Helm Repo') {
      steps{
        script {
          sh "helm repo update"
        }
      }
    }
    stage('List Local Helm Repo') {
      steps{
        script {
          sh "helm search helm-charts"
        }
      }
    }
    stage('Install Heml Chart') {
      steps{
        sh "helm install mynode helm-charts/mynode"
      }
    }
  }
}