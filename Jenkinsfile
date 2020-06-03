pipeline {
   agent any

   stages {
      stage('Version Verification Stage') {
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
             git branch: "development",
                url:'https://github.com/mkumarroy1991/devops-demo-external.git'
             sh 'ls'   
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
            sh 'gcloud container clusters get-credentials deloitte-drifters-demo-cluster --zone us-central1-a --project dtc-user21'
            
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
