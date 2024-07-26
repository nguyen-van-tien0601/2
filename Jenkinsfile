pipeline {
    agent any
    stages {
        stage('SAST') {
            steps {
                sh "semgrep ci"
            }
        }
        
        stage('SCA') {
            steps {
                sh 'trivy fs --skip-db-update --offline-scan -f cyclonedx -o trivy_report.json .' 
            }
        }
        
        stage('Publish to Dependency Track') {
            steps {
                dependencyTrackPublisher(
                    projectName: 'dso-demo',
                    projectVersion: 'dev',
                    artifact: 'trivy_report.json',
                    autoCreateProjects: true,
                    synchronous: true, // Thêm tham số synchronous với giá trị true
                    dependencyTrackApiKey: credentials('dependency-track-api-key'),
                    dependencyTrackUrl: 'http://localhost:8080/'
                )
            }
        }
    }
}
