pipeline {
    agent any 
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

     
       stage('Docker Login'){
    steps {
        
        script{
                   withCredentials([string(credentialsId: 'dockerhub-pwd')]) {
                   sh 'docker login -u hamzaemi -p Hamza1215 '

}

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
