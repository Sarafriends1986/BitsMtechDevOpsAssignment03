node {
   //def mvnHome = tool 'M3'

   stage('Checkout Code') { 
      //git 'git@github.com:Sarafriends1986/java-maven-mathcalculator-web-app.git'
	  checkout scm
   }
   stage('Maven Test') {
      if (isUnix()) {
         //sh "'${mvnHome}/bin/mvn' clean test"
		 sh 'echo "JUnit Test..."'
		 sh 'pwd'
		 sh '/opt/apache-maven-3.8.5/bin/mvn package -Dmaven.test.skip=true'
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
   stage('Code Build') {
      if (isUnix()) {
         //sh "'${mvnHome}/bin/mvn' verify"
		 sh"echo 'Code Build...'"
		 sh '/opt/apache-maven-3.8.5/bin/mvn package -Dmaven.test.skip=true"
		 sh"ls -ltr"
      } else {
        // bat(/"${mvnHome}\bin\mvn" verify/)
      }
   }
   /*
   stage('Deploy') {
      timeout(time: 10, unit: 'MINUTES') {
           input message: 'Deploy this web app to production ?'
      }
      echo 'Deploy...'
   } */
   stage('Deploy') {
      
      cd target
	  ls -ltr
   }
}
   
