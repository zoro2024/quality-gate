node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def mvn = tool 'Default Maven'; 
    withSonarQubeEnv() { 
      sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=quality-gate -Dsonar.projectName='quality-gate'"
    }           // -Dsonar.projectKey and -Dsonar.projectName options specify the project key and name in SonarQube
  }
  stage('Quality Gate Check') {
    timeout(time: 2, unit: 'MINUTES') { // sets a timeout for this stage to 2 minutes. If the stage takes longer than 2 minutes, it will be aborted.
      def qg = waitForQualityGate() // This line waits for the SonarQube analysis to complete and retrieves the quality gate status.
      if (qg.status != 'OK') {
        error "Pipeline aborted due to quality gate failure: ${qg.status}"
      }
    }
  }
  stage('Post-Quality Gate') { // Use this stage, for example, to check the results of what happens when the quality gate fails or passes. 
    echo 'Quality gate passed' // Ideally, at this stage, there should be a command to build artifacts or perform any other tests you want on the code.
  }
}
