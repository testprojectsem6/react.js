pipeline {


  environment {
    dockerimagename = "sem6persproject/react-app"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/testprojectsem6/react.js/']]])
      }
    }
  
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      steps{
        script{
          withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
           sh 'docker login -u sem6persproject -p ${dockerhubpwd}'
           
             }
           sh 'docker push sem6persproject/react-app:latest'
         }  
 
        }
    }
      
    

    stage('Deploy to k8s'){
        steps{
            script{
                kubernetesDeploy (configs: 'deployment.yaml',kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
    }
}
