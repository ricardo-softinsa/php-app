node {
  stage('SCM') {
    git 'https://github.com/ricardo-softinsa/php-app.git'
  }
  stage('SonarQube analysis') {
    // requires SonarQube Scanner 2.8+
    def scannerHome = tool 'Scanner';
    withSonarQubeEnv('SonarServer') {
      bat "\"${scannerHome}/bin/sonar-scanner\""
    }
  }
 stage('Send Slack Notification'){
	def externalMethod = load("slackNotifications.groovy");
	 externalMethod.call(currentBuild.currentResult);
 }
  stage("SonarQube Quality Gate") { 
	timeout(time: 1, unit: 'MINUTES') { 
	   def qg = waitForQualityGate() 
	   def slackMet = load("slackNotifications.groovy");
	   if(qg.status == "ERROR"){
			echo "Failed Quality Gates";
		   	slackMet.afterQG(qg.status);
			error "Pipeline aborted due to quality gate failure: ${qg.status}"
	   }
	   if (qg.status == 'OK') {
		 echo "Passed Quality Gates!";
		 slackMet.afterQG(qg.status);
	   }
	   echo "Status: ";
	   echo qg.status;
	}
  }
  stage("Cloud Push"){
		echo "Pushing to Cloud...";
  }
}
