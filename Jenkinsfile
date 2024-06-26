node {
  stage('SCM') {
    checkout scm
  }
  
  stage('SonarQube Analysis') {
    def mvn = tool 'Default Maven';
    withSonarQubeEnv('SonarQube') {
      sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=quality-gate -Dsonar.projectName='quality-gate'"
    }
  }
  
  stage('Quality Gate') {
    timeout(time: 1, unit: 'HOURS') {
      waitForQualityGate(abortPipeline: true)
    }
  }
  
  stage('Build') {
    // Your build steps go here
    echo 'Build stage - success'
  }
}
