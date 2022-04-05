/* Devops assignment03 */

/* build in Master node */
//node{
/* build in Slave node*/
node('Slave') {
   
   
   stage('Setup parameters') {
      
	  properties([parameters([string(defaultValue: '1.01.001', description: 'appversion x.xx.xxx', name: 'appversion', trim: true), string(defaultValue: '13.229.146.250', description: 'AWS EC2 Private IP of SonarQube server.', name: 'sonarqubeip', trim: true), booleanParam(defaultValue: false, description: 'mark true for app deploy to Prod env.', name: 'deployprod')])])
	  sh 'echo "Setup Parameter...${appversion} ${sonarqubeip} ${deployprod}" '
	
   }

   stage('Checkout Code') { 
     
	  checkout scm
   }
   
   stage('Maven Test') {
      if (isUnix()) {
         
		 sh 'echo "Maven Test..."'
		 sh 'pwd'
		 sh '/opt/apache-maven-3.8.5/bin/mvn clean test'
		 
      } else {
         //bat(/"mvn" clean test/)
      }
   }

   stage('SonarQube Code Scan') {
      if (isUnix()) {
        /* change the private IP of SonarQube EC2 instance */
		
		 sh "echo 'Code Build...'"
		 sh '/opt/apache-maven-3.8.5/bin/mvn sonar:sonar org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar   -Dsonar.projectKey=demoapp-project   -Dsonar.host.url=http://172.31.2.41:9000   -Dsonar.login=621cfbbfce8819d30697733e2eedf547ff13eaa9'
		 
		 
      } else {
        // bat(/"mvn" verify/)
      }
   }
   
   stage('Code Build') {
      if (isUnix()) {
        
		 sh "echo 'Code Build...'"
		 sh '/opt/apache-maven-3.8.5/bin/mvn package -Dmaven.test.skip=true'
		 
		 sh 'cp target/*.war api_${appversion}.war'
		 
      } else {
        // bat(/"mvn" verify/)
      }
   }
   
   stage('Artifact upload to AWS S3') {
   
      sh "echo 'Artifact upload to AWS S3...'"
	  
      s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'mybitsdevops', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'ap-southeast-1', showDirectlyInBrowser: false, sourceFile: 'api_*.war', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'mybitsdevops', userMetadata: []
	  
	  sh 'rm -rf api_${appversion}.war'
   }

   stage('Deploy Stag env') {
   
      sh "echo 'Staging Deploy the App...'"    
		withAWS(credentials: 'AwsUserKey', endpointUrl: 'http://mybitsdevops.s3-website.ap-southeast-1.amazonaws.com', region: 'ap-southeast-1') {
		// some block
		s3Download(file: 'api_1.01.005.war', bucket: 'mybitsdevops', path: '/root')
		}
      
	  sh 'ls -ltr'
   }
   
   stage('Deploy Prod env') {
   
      sh "echo 'Prod Deploy the App...'"  
	  sh "echo sh deployprod is ${params.deployprod}"
      if (params.deployprod) { 
	  print "Proceed with Prod Deploy..." 
	  } else {
	  print "Not to Proceed with Prod Deploy..." 
	  }
      
	  sh 'ls -ltr'
   }
}
   
