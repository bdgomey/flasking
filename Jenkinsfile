pipeline {

  environment {
    registry = 'bjgomes/flask_app'
    registryCredentials = 'docker'
    cluster_name = 'skillstorm'
    namespace = 'bgomes'
  }
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
    stage('Build Stage') {
      steps {
        script {
          dockerImage = docker.build(registry)
        }
      }
    }
    stage('Deploy Stage') {
      steps {
        script {
          docker.withRegistry('', registryCredentials) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Kubernetes') {
      steps {
        withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'AWS', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
          sh "aws eks update-kubeconfig --region us-east-1 --name ${cluster_name}"
          script{
            try{
            sh "kubectl create namespace ${namespace}"
            }catch (Exception e) {
              echo "Exception handled"
            }
          }
          sh "kubectl apply -f deployment.yaml -n ${namespace}"
          sh "kubectl -n ${namespace} rollout restart deployment flaskcontainer"
        }
      }
    }
  }
}