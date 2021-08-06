// prerequisites: a nodejs app must be deployed inside a kubernetes cluster
// TODO: look for all instances of [] and replace all instances of 
//       the 'variables' with actual values 
// variables:
//      https://github.com/vitoscalleta/Practice_External.git
//      master
//      dtcaugust2021-321
//      gcr.io/dtcaugust2021-321/internal-image
//      cluster-1 
//      us-central1-c
//      the following values can be found in the yaml:
//      demo-api
//      [CONTAINER_NAME] (in the template/spec section of the deployment)

pipeline {
    agent any 
    stages {
        stage('Stage 1') {
            steps {
                echo 'Retrieving source from github' 
                git branch: 'master',
                    url: 'https://github.com/vitoscalleta/Practice_External.git'
                echo 'Did we get the source?' 
                sh 'ls -a'
            }
        }
        stage('Stage 2') {
            steps {
                echo 'workspace and versions' 
                sh 'echo $WORKSPACE'
                sh 'gcloud version'
                sh 'nodejs -v'
                sh 'npm -v'
        
            }
        }        
         stage('Stage 3') {
            environment {
                PORT = 8081
            }
            steps {
                echo 'install dependencies' 
                sh 'npm install'
                echo 'Run tests'
                sh 'npm test'
        
            }
        }        
         stage('Stage 4') {
            steps {
                echo "build id = ${env.BUILD_ID}"
                echo 'Tests passed on to build Docker container'
                sh "gcloud builds submit -t gcr.io/dtcaugust2021-321/gcr.io/dtcaugust2021-321/internal-image:v2.${env.BUILD_ID} ."
            }
        }        
         stage('Stage 5') {
            steps {
                echo 'Get cluster credentials'
                sh 'gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project dtcaugust2021-321'
                echo 'Update the image'
                echo "gcr.io/dtcaugust2021-321/gcr.io/dtcaugust2021-321/internal-image:2.${env.BUILD_ID}"
                sh "kubectl set image deployment/demo-api demo-api=gcr.io/dtcaugust2021-321/gcr.io/dtcaugust2021-321/internal-image:v2.${env.BUILD_ID} --record"
            }
        }
    }
}

