node {
  stage('SCM') {
    echo 'Starting SCM stage...'
    checkout scm
    echo 'Finished SCM stage.'
  }
  
  stage('SonarQube Analysis') {
    echo 'Starting SonarQube Analysis stage...'
    def mvn = tool 'Default Maven';
    withSonarQubeEnv('SonarQube') {
      sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=poc -Dsonar.projectName='poc'"
    }
    echo 'Finished SonarQube Analysis stage.'
  }

  stage('Quality Gate') {
    echo 'Starting Quality Gate stage...'
    timeout(time: 5, unit: 'MINUTES') { // Reduced timeout for debugging
      def qg = waitForQualityGate()
      echo "Quality Gate status: ${qg.status}"
      if (qg.status != 'OK') {
        error "Pipeline aborted due to quality gate failure: ${qg.status}"
      }
    }
    echo 'Finished Quality Gate stage.'
  }

  stage('Post-Quality Gate') {
    echo 'Starting Post-Quality Gate stage...'
    echo 'Checking will this pipeline fail due to quality gate failure'
    echo 'Finished Post-Quality Gate stage.'
  }
}
