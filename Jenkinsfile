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
                withCredentials([string(credentialsId: 'dependency-track-api-key', variable: 'DT_API_KEY')]) {
                    dependencyTrackPublisher(
                        projectName: 'dso-demo',
                        projectVersion: 'dev',
                        artifact: 'trivy_report.json',
                        autoCreateProjects: true,
                        synchronous: true,
                        dependencyTrackApiKey: DT_API_KEY,  // Sử dụng biến môi trường ở đây
                        dependencyTrackUrl: 'http://localhost:8080/api/v1/'
                    )
                }
            }
        }
    }
}
