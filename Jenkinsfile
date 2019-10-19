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
      echo 'Waiting 2 minutes for deployment to complete prior starting packaging'
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
          sh "/usr/local/bin/helm init --client-only"
          sh "/usr/local/bin/helm package mynode"
        }
      }
    }
    stage('Update Repo Index and Commit to Github') {
      steps{
        script {
          sh "/usr/local/bin/helm repo index ."
          sh "git add ."
          sh "git commit -m 'Commiting updated Package and Index from Jenkins'"
        }
      }
    }

    stage ("Waiting to Make sure Helm Repo Can be pulled in") {
      steps{
        echo 'Waiting 1 minute for deployment to complete prior starting again'
        sleep 60 // seconds
      }
    }
    stage('Update Local Helm Repo') {
      steps{
        script {
          sh "/usr/local/bin/helm repo update"
        }
      }
    }
    stage('List Local Helm Repo') {
      steps{
        script {
          sh "/usr/local/bin/helm search helm-charts"
        }
      }
    }
    stage('Install Heml Chart') {
      steps{
        sh "/usr/local/bin/helm install mynode helm-charts/mynode"
      }
    }
  }
}