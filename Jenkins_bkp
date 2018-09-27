#!groovy
def app
pipeline {

agent any
tools {
   maven 'maven3'
}

stages {
  
   stage('Checkout'){
	 steps {
		sh "git checkout ${env.BRANCH_NAME}"
	 }
   }
   stage ('Java Build') {
	 steps {
	   sh 'mvn clean install -DskipTests'
	   //sh 'mvn clean package -U'
	 }
   }
   stage('Sonar') {
	 steps {
			 //sh "whoami"
			 //sh "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.3.0.603:sonar -Dsonar.host.url=http://sonar-devel.local"
		   
		// sh 'mvn sonar:sonar -Dsonar.host.url=http://192.168.41.56:9000 -Dsonar.login=6d9caf1ec7a57e01ed5d33fabc3c4577ba1c9e50'
		   sh 'mvn sonar:sonar -Dsonar.host.url=http://192.168.41.56:9000 -Dsonar.login=671b5aa333b894f79e6cac7ca4ccde2819e81628'
	  }
  }
  
  stage('Create Docker Image') {
	  steps{
		//sh "sudo docker stop devops-poc/pipeline:latest"
		//sh "sudo docker rm devops-poc/pipeline:latest"
		sh "sudo docker build -t devops-poc/pipeline:latest ."
	   }
   }

  stage('Run Docker Image') {
					 
	  steps{
	
	   //sh "sudo docker run -p8082:8080  devops-poc-${env.VERSION_NUMBER}/pipeline:latest"
	   sh """
                    sudo docker run -d \
                        -p8082:9090 \
                        -v /var/run/docker.sock:/var/run/docker.sock:ro \
                        -e TIMEOUT=30 \
                        devops-poc/pipeline:latest
            """                        
		 }
	 
	}
  //stage('Run Smoke Tests') {
	//steps {
	  //node('master') {
		//    build job: 'smoketest'
		  //}
	//}
  //}

}
}

