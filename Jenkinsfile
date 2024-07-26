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
       stage('Publish to Dependency Track') {
            steps {
        // Sử dụng dependencyTrackPublisher để đẩy báo cáo lên Dependency Track
                dependencyTrackPublisher(
                    projectName: dso-demo,
                    projectVersion: dev,
                    artifact: 'trivy_report.json',
                    artifactType: 'JSON',
                    autoCreateProject: true,
                    dependencyTrackApiKey: credentials('dependency-track-api-key'),
                    dependencyTrackUrl: 'http://localhost:8080/'
                )
    }
  }
}
}
