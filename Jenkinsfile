pipeline {
    agent any

    stages {
        stage('Build Dockerfile') {
            steps {
              sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
              sh 'docker image tag $JOB_NAME:v1.$BUILD_ID 8875022556/$JOB_NAME:v1.$BUILD_ID' 
              sh 'docker image tag $JOB_NAME:v1.$BUILD_ID 8875022556/$JOB_NAME:latest'        
            }
        }
        stage('Push Image To Docker HUB'){
            steps{
           withCredentials([string(credentialsId: 'docker', variable: 'docker')]) {
    // some block
                sh 'docker login -u 8875022556 -p ${docker} '
                sh 'docker image push 8875022556/$JOB_NAME:v1.$BUILD_ID'
                sh 'docker image push 8875022556/$JOB_NAME:latest'
                sh 'docker image rmi $JOB_NAME:v1.$BUILD_ID 8875022556/$JOB_NAME:v1.$BUILD_ID 8875022556/$JOB_NAME:latest '
                  }
                }
            }
        stage('Deployment of Docker Container'){
            steps
            sshagent(['new']) {
                    sh "ssh -o StrictHostKeyChecking=no root@172.31.44.141 ${new}"
              }  
            }
        }
      }
    }
