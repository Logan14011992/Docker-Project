pipeline {
environment {
    registry = "10.138.0.3:5001/Logan14011992/flask"
    registry_mysql = "10.138.0.3:5001/Logan14011992/mysql"
    dockerImage = ""
  }
agent any
    stages {
  stage('Checkout Source') {
      steps {
        git 'https://github.com/Logan14011992/Docker-Project.git'
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
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }
stage('current') {
      steps {
        dir("${env.WORKSPACE}/mysql"){
          sh "pwd"
          }
      }
   }
      stage('Build mysql image') {
     steps {
       sh 'docker build -t "10.138.0.3:5001/Logan14011992/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
        sh 'docker push "10.138.0.3:5001/Logan14011992/mysql:$BUILD_NUMBER"'
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
