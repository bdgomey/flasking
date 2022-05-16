pipeline {
  agent {
    node {
      label 'docker'
    }

  }
  stages {
    stage('git') {
      steps {
        git(url: 'https://github.com/bdgomey/flasking.git', branch: 'main')
      }
    }

    stage('Build') {
      steps {
        sh 'docker build -t flask_app .'
      }
    }

    stage('Docker Login') {
      steps {
        sh 'docker login -u bjgomes -p 147f97ff-be6a-47cc-8719-81145e357ce7'
      }
    }

    stage('Docker Push') {
      steps {
        sh 'docker push flask_app:latest'
      }
    }

  }
}