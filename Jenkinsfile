pipeline {
   agent any

   environment {
     // You must set the following environment variables
     // ORGANIZATION_NAME
     // YOUR_DOCKERHUB_USERNAME (it doesn't matter if you don't have one)

     SERVICE_NAME = "fleetman-mongodb"     
     REPOSITORY_TAG="${YOUR_DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
   }

   stages {
      stage('Preparation') {
         steps {
            cleanWs()
            git credentialsId: 'GitHub', url: "https://github.com/${ORGANIZATION_NAME}/${SERVICE_NAME}"
         }
      }
      stage('Build') {
         steps {
            sh '''echo No build required for Mongodb'''
         }
      }

      stage('Build and Push Image') {
         steps {
           sh 'echo No docker image for Mongodb'
         }
      }
      
      stage("Docker Push") {
         steps {
           withCredentials([string(credentialsId: 'DockerHub_credentials', variable: 'DockerHub_credentials')]) {
             sh "docker login -u hashimabd -p ${DockerHub_credentials}"
              sh "docker push ${REPOSITORY_TAG}"
           }
         }   
      }

      stage("Deploy to Kubernetes") {
         steps {
           kubernetesDeploy(
            configs:'deploy.yaml',
            kubeconfigId: 'kubernetes_cluster_config',
            enableConfigSubstitution: true
            )
         }   
      }
   }
}
