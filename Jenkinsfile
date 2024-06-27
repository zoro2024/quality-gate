node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def mvn = tool 'Default Maven';
    withSonarQubeEnv() {
      sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=quality-gate -Dsonar.projectName='quality-gate'"
    }
  }
  stage('Quality Gate') {
    timeout(time: 1, unit: 'HOURS') {
      def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
      if (qg.status != 'OK') {
        error "Pipeline aborted due to quality gate failure: ${qg.status}"
      }
    }
  }
  stage('Post-Quality Gate') {
    echo 'Quality gate passed'
  }
}
