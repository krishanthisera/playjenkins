pipeline {
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git 'https://github.com/krishanthisera/playjenkins.git'
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
      steps {
        script {
          docker.withRegistry( "https://shipping.bizkt.com.au" ) {
            dockerImage.push()
          }
        }

      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "kube-key-01")
        }

      }
    }

  }
  environment {
    registry = 'shipping.bizkt.com.au:443/justme/myweb'
    dockerImage = ''
  }
}
