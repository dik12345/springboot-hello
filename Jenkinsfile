pipeline {
    agent any
    tools {
  maven 'apache maven3'
}

    stages {
        stage('Compile and Clean') { 
            steps {
                // Run Maven on a Unix agent.
              
                sh "mvn clean compile"
            }
        }
        stage('deploy') {
            steps {
                sh "mvn package"  
            }
        }
        stage('Build docker image') {
            steps {
                echo "Hello web Application"
                sh 'ls'
                sh 'sudo docker build -t shine1234/docker_jenkins_springboot:${BUILD_NUMBER} .'
           }
        }
        stage('Docker Login'){
            
            steps {
               withCredentials([string(credentialsId: 'dockerid', variable: 'DOCKERPWD')]) {
		    
                   sh "sudo docker login -u shine1234 -p ${DOCKERPWD}"
                }
            }                
        }
	stage('Docker Push'){
            steps {
                sh 'sudo docker push shine1234/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }
        stage('Docker deploy'){
            steps {
               
                sh 'sudo docker run -itd -p  8081:8080 shine1234/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }
 	stage('Archving') { 
            steps {
                 archiveArtifacts '**/target/*.jar'
            }
        }
    }
}
