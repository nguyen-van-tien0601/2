pipeline {
    agent any
    stages {
        stage('SAST') {
            steps {
               sh "semgrep ci"
            }
        }
        
        stage ('SCA') {
            steps {
                sh 'trivy fs --skip-db-update --offline-scan -f cyclonedx -o trivy_report.json .' 
            }
        }
  }
        
