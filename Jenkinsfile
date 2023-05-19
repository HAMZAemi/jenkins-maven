pipeline {
    agent any
    
    environment {
        DOCKERHUB_USERNAME = credentials('76e78526-bcfb-4935-b8a5-c62e5a32bb0e').username
        DOCKERHUB_PASSWORD = credentials('76e78526-bcfb-4935-b8a5-c62e5a32bb0e').password
    }
    
    stages {
        stage('Build and push Docker image') {
            steps {
                sh "docker build -t myimage:${BUILD_NUMBER} ."
                sh "echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin"
                sh "docker tag myimage:${BUILD_NUMBER} myusername/myimage:${BUILD_NUMBER}"
                sh "docker push myusername/myimage:${BUILD_NUMBER}"
            }
        }
    }
}
