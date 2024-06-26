node {
    stage('SCM') {
        checkout scm
    }
    
    stage('SonarQube Analysis') {
        def mvnHome = tool 'Default Maven'
        withSonarQubeEnv() {
            sh "${mvnHome}/bin/mvn clean verify sonar:sonar"
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
        echo 'checking will this pipeline fail due to quality gate failure'
    }
}
