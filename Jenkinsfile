#!groovy

def repo = "https://github.com/PramodSappy/expresso-testing.git"
def branch = "master"
pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        script {
          git branch: branch, credentialsId: '', url: repo
        }
      }
    }
    stage('Build artifacts') {
      agent { label "GRADLE" }
      steps {
        script {
          try {
            sh '$GRADLE4/bin/gradle clean assemble'
          } catch (e) {
            echo 'Build Failed....'
            throw e
          }
        }
      }
    }
    stage('Test') {
      agent { label "GRADLE" }
      steps {
        script {
          try {
            sh '$GRADLE4/bin/gradle test'
          } catch (e) {
             echo 'Build Failed....'
            throw e
          }
        }
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploying....'
      }
    }
  }
}