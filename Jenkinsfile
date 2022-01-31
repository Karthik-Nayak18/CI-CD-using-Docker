pipeline {
    agent any
	
	  tools
    {
       maven "Maven"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/Karthik-Nayak18/CI-CD-using-Docker.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t samplewebapp:latest .' 
		
                sh 'docker tag samplewebapp:latest 141325/samplewebapp:latest'
                //sh 'docker tag samplewebapp 141325/samplewebapp:$BUILD_NUMBER'
               
          }
        }
     
     
    stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{
                sh "docker run -d -p 8001:8080 141325/samplewebapp:latest"
 
            }
        }
	 
stage('Publish image to Docker Hub') {
          
            steps {
        withDockerRegistry([ credentialsId: "dockerHub", url: "https://hub.docker.com/repository/docker/141325/samplewebapp" ]) {
          sh  'docker push 141325/samplewebapp:latest'
        //  sh  'docker push 141325/samplewebapp:$BUILD_NUMBER' 
        }
                  
          }
        }
	 
 stage('Run Docker container on remote hosts') {
             
            steps {
                sh "docker -H ssh://ec2-user@3.111.58.169 run -d -p 8003:8080 141325/samplewebapp:latest"
 
            }
        }
    }
	}
    
