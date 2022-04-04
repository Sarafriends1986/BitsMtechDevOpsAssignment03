node {
   //def mvnHome = tool 'M3'
   //agent { label 'master' }
   stage('Setup parameters') {
      //properties([parameters([string(description: 'IP of SonarQube server', name: 'sonarqubeip', trim: true)])])
	  properties([parameters([string(defaultValue: '1.01.001', description: 'appversion x.xx.xxx', name: 'appversion', trim: true), string(defaultValue: '13.229.146.250', description: 'IP of SonarQube server', name: 'sonarqubeip', trim: true)])])
	  sh 'echo "Setup Parameter...${appversion} ${sonarqubeip}" ${appversion} ${sonarqubeip}'
	
   }

   stage('Checkout Code') { 
      //git 'git@github.com:Sarafriends1986/java-maven-mathcalculator-web-app.git'
	  checkout scm
   }
   stage('Maven Test') {
      if (isUnix()) {
         //sh "'${mvnHome}/bin/mvn' clean test"
		 sh 'echo "JUnit Test...${appversion}" ${appversion} ${sonarqubeip}'
		 sh 'pwd'
		 sh '/opt/apache-maven-3.8.5/bin/mvn clean test'
		 //sh"ls -ltr"
      } else {
         //bat(/"${mvnHome}\bin\mvn" clean test/)
      }
   }
 /*
   stage('Performance Test') {
      if (isUnix()) {
         //sh "'${mvnHome}/bin/mvn' cargo:start verify cargo:stop"
		 sh"echo 'Performance Test...'"
		 sh"ls -ltr"
      } else {
         //bat(/"${mvnHome}\bin\mvn" cargo:start verify cargo:stop/)
      }
   }
  */
   stage('SonarQube Code Scan') {
      if (isUnix()) {
         //sh "'${mvnHome}/bin/mvn' verify"
		 sh"echo 'Code Build...${appversion}'${appversion}"
		 sh '/opt/apache-maven-3.8.5/bin/mvn sonar:sonar org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar   -Dsonar.projectKey=demoapp-project   -Dsonar.host.url=http://172.31.2.41:9000   -Dsonar.login=621cfbbfce8819d30697733e2eedf547ff13eaa9'

		 
		 
      } else {
        // bat(/"${mvnHome}\bin\mvn" verify/)
      }
   }stage('Code Build') {
      if (isUnix()) {
         //sh "'${mvnHome}/bin/mvn' verify"
		 sh"echo 'Code Build...${appversion}'${appversion} ${sonarqubeip}"
		 sh '/opt/apache-maven-3.8.5/bin/mvn package -Dmaven.test.skip=true'
		 sh 'ls -ltr'
		 sh 'cp target/*.war api_${appversion}.war'
		 
      } else {
        // bat(/"${mvnHome}\bin\mvn" verify/)
      }
   }
   
   stage('Artifact upload to AWS S3') {
      s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'mybitsdevops', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'ap-southeast-1', showDirectlyInBrowser: false, sourceFile: 'api_*.war', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'mybitsdevops', userMetadata: []
   }
   /*
   stage('Deploy') {
      timeout(time: 10, unit: 'MINUTES') {
           input message: 'Deploy this web app to production ?'
      }
      echo 'Deploy...'
   } */
   stage('Deploy') {
      
      sh 'cd target'
	  sh 'ls -ltr'
   }
}
   
