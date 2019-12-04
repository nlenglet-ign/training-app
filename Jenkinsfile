node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/nlenglet-ign/training-app.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'Maven'
   }
   stage('Sonarqube') {
       if(isUnix()) {
           sh "'${mvnHome}/bin/mvn' -B sonar:sonar"
       } else {
           bat (/"${mvnHome}\bin\mvn" -B sonar:sonar/)
       }
   }
   stage('Build') {
      // Run the maven build
      withEnv(["MVN_HOME=$mvnHome"]) {
         if (isUnix()) {
            sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package'
         } else {
            bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean install/)
         }
      }
   }   
   stage('Test') {
       if(isUnix()) {
           sh "'${mvnHome}/bin/mvn' test"
       } else {
           bat (/"${mvnHome}\bin\mvn" test/)
       }
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archiveArtifacts 'target/*.jar'
   }
   stage('Execute') {
        if(isUnix()) {
            sh "java -jar target/*.jar"    
        } else {
            bat (/java -jar target\/training-app-1.0-SNAPSHOT-jar-with-dependencies.jar/)   
        }
   }
   stage('Public artefacts') {
      if(isUnix()) {
          sh "'${mvnHome}/bin/mvn' -DdeployOnly deploy"
      } else {
          bat (/"${mvnHome}\bin\mvn" -DdeployOnly only -s "C:\integ_continue\apache-maven-3.6.3-bin\apache-maven-3.6.3\conf\settings.xml" /)
      }

   }
}