pipeline {
    agent any
    
    stages {
        stage('SonarQube Code Analysis') {
            steps {
                dir("${WORKSPACE}") {
                    script {
                        def scannerHome = tool name: 'scanner-name', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                        withSonarQubeEnv('sonar') {
                            sh "echo \$PWD"  // Ensure correct shell variable for current directory
                            sh "${scannerHome}/bin/sonar-scanner"
                        }
                    }
                }
            }
        }
        
        stage("SonarQube Quality Gate Check") {
            steps {
                script {
                    def qualityGate = waitForQualityGate()
                    
                    if (qualityGate.status != 'OK') {
                        echo "${qualityGate.status}"
                        error "Quality Gate failed: ${qualityGate.status}"  // Corrected variable name
                    } else {
                        echo "${qualityGate.status}"
                        echo "SonarQube Quality Gates Passed"
                    }
                }
            }
        }
    }
}
