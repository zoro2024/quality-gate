pipeline {
    agent any
    
    stages {
        stage('SCM Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('SonarQube Analysis') {
            environment {
                scannerHome = tool 'Default Maven'
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "${scannerHome}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=quality-gate -Dsonar.projectName='quality-gate'"
                }
            }
        }
    }
}
