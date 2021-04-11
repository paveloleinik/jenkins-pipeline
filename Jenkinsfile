pipeline {
  agent any
  triggers {
    pollSCM('*/5 * * * *')
  }
  stages{
       stage ('Build'){
        steps {
          sh 'mvn clean package'
        }
         post {
           success {
             echo 'Archiving...'
             archiveArtifacts artifacts:'**/target/*.war'
           }
         }
       }
       stage ('Deployments') {
         parallel{
           stage ('Deploy to Staging'){
             steps {
               build job:'ITVDN-deploy_to_stage'
             }
           }
           stage ('Deploy to prod') {
             steps {
               timeout(time:5, unit: 'DAYS') {
                 input message: "Approve Prod deployment"
               }
               build job:'ITVDN-deploy_to_prod'
             }     
           }
         }
       }
    }
} 
  
