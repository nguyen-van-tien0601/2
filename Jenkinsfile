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
                withCredentials([string(credentialsId: 'odt_3XTFKJ3TZSA5B5JRMxI8UZbarZ8B1XsJ', variable: 'API_KEY')]) {
                    dependencyTrackPublisher(
                        artifact: 'trivy_report.json',
                        projectName: 'my-project',
                        projectVersion: 'my-version',
                        synchronous: true,
                        dependencyTrackApiKey: API_KEY,
                        projectId: 'your-project-id',  // Thay đổi 'your-project-id' thành ID dự án thực tế
                        autoCreateProjects: false,    // Đặt thành false để chỉ định projectId
                        projectProperties: [
                            tags: ['tag1', 'tag2'],
                            swidTagId: 'my swid tag',
                            group: 'my group',
                            parentId: 'parent-uuid'
                        ]
                    )
                }
            }
        }
    }
}
