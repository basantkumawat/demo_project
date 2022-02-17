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
        stage('Run Docker container on Jenkins Agent'){
            steps{
                sh "docker run -d -p 8000:80 8875022556/demo_project:latest"
            }
        }
        stage('Run Docker container on remote hosts'){
            steps{
                sh "docker -H ssh://root@172.31.28.25 run -d -p 8000:80 8875022556/demo_project:latest"
            }
        }
      }
    }
