node {
	
    def application = "devopsexample"
    
    def dockerhubaccountid = "ak2277"
	   
    def mvnHome = tool 'maven-3.5.2'

    def dockerImage
 
    def dockerImageTag = "${dockerhubaccountid}/${application}:${env.BUILD_NUMBER}"
    
    stage('Clone Repo') { 
      git url:'https://github.com/AkshaySuthar4592/Devops-Integration',branch:'main' //update your forked repo           
      mvnHome = tool 'maven-3.5.2'
    }    
  
    stage('Build Project') {
      sh "'${mvnHome}/bin/mvn' clean install"
    }
		
    stage('Build Docker Image with new code') {
      dockerImage = docker.build("${dockerhubaccountid}/${application}:${env.BUILD_NUMBER}")
    }
	
    stage('Push Image to Remote Repo'){
	 echo "Docker Image Tag Name ---> ${dockerImageTag}"
	     docker.withRegistry('', 'dockerHub') {
             dockerImage.push("${env.BUILD_NUMBER}")
             dockerImage.push("latest")
            }
	}
   
   stage('Remove running container with old code'){
	  sh "docker rm -f \$(docker ps -a -f name=devopsexample -q) || true"   
	       
    }
	
    stage('Deploy Docker Image with new changes'){
	  sh "docker run --name devopsexample -d -p 2222:2222 ${dockerhubaccountid}/${application}:${env.BUILD_NUMBER}"  
	  
    }
	
    stage('Remove old images') {
		sh("docker rmi ${dockerhubaccountid}/${application}:latest -f")
   }
}
