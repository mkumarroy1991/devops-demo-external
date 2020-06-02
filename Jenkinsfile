pipeline {
   agent any

   stages {
      stage('Stage 1 - Version Checks') {
         steps {
            echo 'Checking Versions...'
            sh 'docker --version'
            sh 'terraform version'
            sh 'git version'
            sh 'node -v'
            sh 'npm -v'
            sh 'gcloud version'
            
         }
      }
      stage('Stage 2 - Cloning GitHub Repo Into Jenkins...') {
         steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '3e7e3ea4-1155-4264-ae65-c90a76ef5173', url: 'https://github.com/mkumarroy1991/devops-demo-external.git']]])
         }
      }
      stage('Stage 3 - Package Application using Docker and storing the image in registry..') {
         steps {
            echo 'Docker Images List'
            sh 'docker images'
            echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
            sh "gcloud builds submit --tag=gcr.io/dtc-user21/jenkins-pipe-external:${env.BUILD_ID} . "
            
         }
      }
      stage('Stage 4 - Deployment - Kubernetes Cluster Resource Creation/Update') {
         steps {
            echo 'Installing Kubernetes Cluster'
            sh 'kubectl version'
            sh 'pwd'
            sh 'kubectl apply -f kubernetes/kube-config-namespaces.yaml'
            sh 'kubectl apply -f kubernetes/kube-config-backend.yaml'
            sh 'kubectl apply -f kubernetes/kube-config-frontend.yaml'
            sh 'kubectl get services -n qa'
            sh 'kubectl get deployments -n qa'
            sh 'kubectl get pods -n qa'
            
            
            
            
         }
      }
   }
   
}

