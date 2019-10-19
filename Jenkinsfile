pipeline {
  environment {
    registry = "cnadella/docker-test"
    registryCredential = 'DockerHub'
    dockerImage = ''
  }
  agent any
  stages {
    stage ("Sleep to Make sure Helm Repo Can be refreshed in") {
      steps{
      echo 'Waiting 5 minutes for deployment to complete prior starting smoke testing'
      sleep 120 // seconds
      }
    }
    stage('Cloning Git') {
      steps {
        checkout scm
      }
    }
    stage('Build helm package') {
      steps{
        script {
          sh "helm package mynode"
        }
      }
    }
    stage('Update Repo Index and Commit to Github') {
      steps{
        script {
          sh "helm repo index ."
          sh "git add ."
          sh "git commit -m 'Commiting updated Package and Index from Jenkins'"
        }
      }
    }

    stage ("Waiting to Make sure Helm Repo Can be pulled in") {
      steps{
        echo 'Waiting 5 minutes for deployment to complete prior starting smoke testing'
        sleep 60 // seconds
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