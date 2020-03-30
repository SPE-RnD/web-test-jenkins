pipeline {

  environment { 
    registry = "192.168.56.140:5000/test/web" //IP & Port Container Private Registry
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') { //Untuk menentukan repository
      steps {
        git 'https://github.com/SPE-RnD/web-test-jenkins.git' //Link repository yang digunakan 
      }
    }

    stage('Build image') { //Untuk membuat image
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER" //Perintah untuk build image
        }
      }
    }

    stage('Push Image') { //Untuk Push Image 
      steps{
        script {
          docker.withRegistry( "" ) {
            dockerImage.push() //Perintah untuk push image sesuai dengan registry yang digunakan 
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig") //Menentukan file deployment kubernetes dan token kubernetes
        }
      }
    }

  }

}
