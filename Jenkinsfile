pipeline {
    agent any 
      environment {
    DOCKERHUB_USERNAME = credentials('dockerhub-creds').username
    DOCKERHUB_PASSWORD = credentials('dockerhub-creds').password
  }
    stages {
        stage('Compile and Clean') { 
            steps {

                sh "mvn clean compile"
            }
        }
        stage('Test') { 
            steps {
                sh "mvn test site"
            }
            
             post {
                always {
                    junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'   
                }
            }     
        }

        stage('deploy') { 
            steps {
                sh "mvn package"
            }
        }


        stage('Build Docker image'){
            steps {
                sh "docker build -t anvbhaskar/docker_jenkins_pipeline:${BUILD_NUMBER} ."
            }
        }

      stage('Push image to Hub'){
            steps {
                sh "echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin"
                sh "docker push anvbhaskar/docker_jenkins_pipeline:${BUILD_NUMBER}"
            }
        }
  
 stage('Docker*'){
            steps {
                sh "docker tag anvbhaskar/docker_jenkins_pipeline:${BUILD_NUMBER} hamzaemi/hamza_el:${BUILD_NUMBER}"
               
            }
        }
        
        stage('Docker Push'){
            steps {
                sh "docker push hamzaemi/hamza_el:${BUILD_NUMBER}"
            }
        }
        
        stage('Docker deploy'){
            steps {
                sh 'docker run -itd -p 8081:8080 anvbhaskar/springboot:0.0.3'
            }
        }

        
        stage('Archving') { 
            steps {
                 archiveArtifacts '**/target/*.jar'
            }
        }
    }
}
