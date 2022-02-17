pipeline {
    agent any

    stages {
        stage('Build Dockerfile') {
            steps {
              sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
              sh 'docker image tag $JOB_NAME:v1.$BUILD_ID network2022/$JOB_NAME:v1.$BUILD_ID' 
              sh 'docker image tag $JOB_NAME:v1.$BUILD_ID network2022/$JOB_NAME:latest'        
            }
        }
        stage('Push Image To Docker HUB'){
            steps{
            withCredentials([string(credentialsId: 'dhubpassword', variable: 'dockerhubpasswd')]) {
    // some block
                sh 'docker login -u network2022 -p ${dhubpassword} '
                sh 'docker image push network2022/$JOB_NAME:v1.$BUILD_ID'
                sh 'docker image push network2022/$JOB_NAME:latest'
                sh 'docker image rmi $JOB_NAME:v1.$BUILD_ID network2022/$JOB_NAME:v1.$BUILD_ID network2022/$JOB_NAME:latest '
}
            }
        }
    }
}
