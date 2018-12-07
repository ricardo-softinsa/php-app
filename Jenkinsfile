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
  stage("SonarQube Quality Gate") { 
	timeout(time: 5, unit: 'MINUTES') { 
	   def qg = waitForQualityGate() 
	   if(qg.status == "ERROR"){
			echo "Failed Quality Gates";
			error "Pipeline aborted due to quality gate failure: ${qg.status}"
	   }
	   if (qg.status == 'OK') {
		 echo "Passed Quality Gates!";
	   }
	   
	}
  }
  stage("Cloud Push"){
		echo "Pushing to Cloud...";
  }
}
