pipeline {
   agent any

   stages {
      stage('Version Checks Stage') {
         steps {
            echo 'Checking Versions...'
            sh 'docker --version'
            sh 'terraform version'
            sh 'git version'
            sh 'node -v'
            sh 'npm -v'
            
         }
      }
      stage('Cloning GitHub Repo Into Jenkins...') {
         steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '3e7e3ea4-1155-4264-ae65-c90a76ef5173', url: 'https://github.com/mkumarroy1991/devops-demo-external.git']]])
         }
      }
      
      stage('Package Application using Docker and storing the image in registry..') {
         steps {
            echo 'Docker Images List'
            sh 'docker images'
            echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
            sh "gcloud builds submit --tag=gcr.io/dtc-user21/jenkins-pipe-external:v1.${env.BUILD_ID} . "
         }
      }
      stage('Deployment - Kubernetes Cluster Resource Creation/Update') {
         steps {
            echo 'Installing Kubernetes Cluster'
            sh 'pwd'
            sh 'ls'
            sh 'gcloud container clusters get-credentials deloitte-drifters-cluster --zone us-central1-a --project dtc-user21'
            sh 'kubectl version'
            sh 'kubectl get namespaces'
            echo 'kubectl set image deployment/deloitte-drifters-frontend deloitte-drifters-frontend=gcr.io/dtc-user21/jenkins-pipe-external:v1.${env.BUILD_ID} --record --namespace=qa'
            sh "kubectl set image deployment/deloitte-drifters-frontend deloitte-drifters-frontend=gcr.io/dtc-user21/jenkins-pipe-external:v1.${env.BUILD_ID} --record --namespace=qa"
            
            sh 'kubectl get deployments -n qa'
            sh 'kubectl get pods -n qa'
            sh 'kubectl get services -n qa'
            
            
            
         }
      }
   }
   
}
