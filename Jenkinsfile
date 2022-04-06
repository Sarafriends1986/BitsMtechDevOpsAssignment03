/* Devops assignment03 */

/* build in Master node */
//node{
/* build in Slave node*/
node('Slave') {
   
   
   stage('Setup parameters') {
   
	   try {
		  
		  properties([parameters([string(defaultValue: '1.01.010', description: 'appversion x.xx.xxx', name: 'appversion', trim: true), string(defaultValue: '172.31.2.41', description: 'AWS EC2 Private IP of SonarQube server.', name: 'sonarqubeip', trim: true), booleanParam(defaultValue: false, description: 'mark true for app deploy to Prod env.', name: 'deployprod')])])
		  
		  sh 'echo "Setup Parameter...${appversion} ${sonarqubeip} ${deployprod}" '
		  
		}catch (err) {
        echo "Caught: ${err}"
        currentBuild.result = 'FAILURE'
		}
	
   }

   stage('Checkout Code') { 
     
	 try {
	  checkout scm
	 }catch (err) {
	  echo "Caught: ${err}"
	  currentBuild.result = 'FAILURE'
	 }
	 
   }
   
   stage('Maven Test') {
   
		try {
		  if (isUnix()) {
			 
			 sh 'echo "Maven Test..."'
			 sh 'pwd'
			 sh '/opt/apache-maven-3.8.5/bin/mvn clean test'
			 
		   } else {
			 //bat(/"mvn" clean test/)
		   }
		   
		}catch (err) {
		 echo "Caught: ${err}"
		 currentBuild.result = 'FAILURE'
	    }
	  
   }

   stage('SonarQube Code Scan') {
   
		try {
		  if (isUnix()) {
			/* change the private IP of SonarQube EC2 instance */
			
			 sh "echo 'Code Build...'"
			 sh '/opt/apache-maven-3.8.5/bin/mvn sonar:sonar org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar   -Dsonar.projectKey=demoapp-project   -Dsonar.host.url=http://${sonarqubeip}:9000   -Dsonar.login=621cfbbfce8819d30697733e2eedf547ff13eaa9'
			 
			 
		  } else {
			// bat(echo 'Code Build...')
		  }
		
		}catch (err) {
		 echo "Caught: ${err}"
		 currentBuild.result = 'FAILURE'
	    }
   }
   
   stage('Code Build') {
   
		try {
		  if (isUnix()) {
			
			 sh "echo 'Code Build...'"
			 sh '/opt/apache-maven-3.8.5/bin/mvn package -Dmaven.test.skip=true'
			 
			 sh 'cp target/*.war api_${appversion}.war'
			 
		  } else {
			// bat(echo 'Code Build...')
		  }
		}catch (err) {
		 echo "Caught: ${err}"
		 currentBuild.result = 'FAILURE'
	    }
   }
   
   stage('Artifact upload to AWS S3') {
   
		try {
		  if (isUnix()) {  
			  sh "echo 'Artifact upload to AWS S3...'"
			  
			  s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'mybitsdevops', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'ap-southeast-1', showDirectlyInBrowser: false, sourceFile: 'api_*.war', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'mybitsdevops', userMetadata: []
			  
			  sh 'rm -rf api_${appversion}.war'
			  
			} else {
			// bat(echo 'Artifact upload to AWS S3...')
		  }
		}catch (err) {
		 echo "Caught: ${err}"
		 currentBuild.result = 'FAILURE'
	    }  
   }

   stage('Deploy Stag env') {  
		
		try {
		  if (isUnix()) { 
			echo 'Deploy Stag env...${appversion}'
			sh 'ansible-playbook deployment.yml -i inventoryhosts --extra-vars "host=stage appversion=${appversion}"'   
		  
		  } else {
			// bat(echo 'Deploy Stag env...${appversion}')
		  }
		}catch (err) {
		 echo "Caught: ${err}"
		 currentBuild.result = 'FAILURE'
	    }  
		
   }
   
   stage('Deploy Prod env') {
	
		try {
		  if (isUnix()) { 
		  
				sh '''#!/bin/sh

					echo "Deploy Prod env... ${deployprod}"

					if [ ${deployprod} == true ]
					then
					   ansible-playbook deployment.yml -i inventoryhosts --extra-vars "host=prod appversion=${appversion}"
					else
					   echo "Not to Proceed with Prod Deploy..."
					fi'''
			  
		  } else {
			// bat(echo 'Deploy Prod env... ${deployprod}')
		  }
		}catch (err) {
		 echo "Caught: ${err}"
		 currentBuild.result = 'FAILURE'
	    }
	  
   }
}
   
