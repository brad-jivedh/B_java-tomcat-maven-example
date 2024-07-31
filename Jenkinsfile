pipeline{
    agent {label 'docker_node_new'}
    environment{
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    stages{
       stage('Git Checkout Stage'){
            steps{
                git branch: 'master', url: 'https://github.com/brad-jivedh/B_java-tomcat-maven-example.git'
            }
         }        
        stage('Build docker Image'){
          steps{
            sh 'docker build -t jivedh2019/assign:IMAGE_TAG .'
          }
        }
        stage('Push To Dockerhub'){
          steps{
            sh 'docker push jivedh2019/assign:IMAGE_TAG'
          }
        }
        stage('Deploy Stage') {
          steps{
                withCredentials([aws(credentialsId: "awsCredentials", region: "ap-south-1")]) {
                    sh 'aws eks --region ap-south-1 update-kubeconfig --name eks-cluster'
                    sh 'helm upgrade --install tomcat-deploy-project ./helm2 -n test'
              }
          } 
        }
    }
}
