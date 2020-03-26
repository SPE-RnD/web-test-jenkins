pipeline {

  environment {
    registry = "192.168.56.140:5000/test/myweb"
    dockerImage = ""
  }

  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git 'https://github.com/SPE-RnD/web-test-jenkins.git'
      }
    }

    stage('Build image') {
      steps {
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }

      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "kubeconfig")
        }

      }
    }

  }
  environment {
    registry = '192.168.56.140:5000/test/myweb'
    dockerImage = ''
  }
}