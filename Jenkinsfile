pipeline {
    agent any
    tools {
  maven 'apache maven3'
}

    stages {
        stage('SCM') {
            steps {
		sh git 'https://github.com/dik12345/springboot-hello.git'
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
                sh 'docker build -t shine1234/docker_jenkins_springboot:${BUILD_NUMBER} .'
           }
        }
        stage('Docker Login'){
            
            steps {
               withCredentials([string(credentialsId: 'dockerid', variable: 'DOCKERPWD')]) {
		    
                   sh "docker login -u shine1234 -p ${DOCKERPWD}"
                }
            }                
        }
	stage('Docker Push'){
            steps {
                sh 'docker push shine1234/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }
        stage('Docker deploy'){
            steps {
               
                sh 'docker run -itd -p  8081:8080 shine1234/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }
 	stage('Archving') { 
            steps {
                 archiveArtifacts '**/target/*.jar'
            }
        }
    }
}
