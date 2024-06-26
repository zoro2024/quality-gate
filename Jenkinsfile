node {
  stage('SCM') {
    checkout scm
  }
  
  stage('SonarQube Analysis') {
    def mvn = tool 'Default Maven';
    withSonarQubeEnv() {
      sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=poc -Dsonar.projectName='poc'"
    }
  }

  stage('Quality Gate') {
    timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
      def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
      if (qg.status != 'OK') {
        error "Pipeline aborted due to quality gate failure: ${qg.status}"
      }
    }
  }

  stage('Post-Quality Gate') {
    echo 'checking will this pipeline fail due to quality gate failure'
  }
}
