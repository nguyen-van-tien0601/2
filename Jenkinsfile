pipeline {
    agent any
    stages {
        stage('SAST') {
            steps {
                // sh "semgrep ci"
                sh "echo 'hello'"
            }
        }
        
        stage('SCA') {
            steps {
                sh 'trivy fs --skip-db-update --offline-scan -f cyclonedx -o trivy_report.json .' 
            }
        }
        
        stage('Publish to Dependency Track') {
            steps {
                withCredentials([string(credentialsId: 'dependency-track-api-key', variable: 'API_KEY')]) {
                    dependencyTrackPublisher(
                        artifact: 'trivy_report.json',
                        projectName: 'my-project',
                        projectVersion: 'my-version',
                        synchronous: true,
                        dependencyTrackApiKey: API_KEY,
                        autoCreateProjects: true
                    )
                    )
                }
            }
        }
    }
}
