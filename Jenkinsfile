node {
    stage('SCM') {
        checkout scm
    }
    
    stage('SonarQube Analysis') {
        def mvnHome = tool 'Default Maven'
        withSonarQubeEnv('SonarQube') {
            sh "${mvnHome}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=quality-gate -Dsonar.projectName='quality-gate'"
        }
    }
}
