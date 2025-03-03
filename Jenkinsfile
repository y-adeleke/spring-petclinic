pipeline {
    agent any

    // Trigger every 3 minutes on Monday (day-of-week 1)
    triggers {
        cron('H/3 * * * 1')
    }

    stages {
        stage('Build') {
            steps {
                // Build the project and generate an artifact
                sh 'mvn clean package'
            }
        }
        stage('Test with Jacoco') {
            steps {
                // Run tests and generate the Jacoco report
                sh 'mvn test jacoco:report'
            }
            post {
                always {
                    // Publish the Jacoco report as an HTML report in Jenkins
                    publishHTML(target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'target/site/jacoco',
                        reportFiles: 'index.html',
                        reportName: "Jacoco Coverage Report"
                    ])
                }
            }
        }
    }

    post {
        success {
            // Archive the generated artifact (e.g., jar file)
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
    }
}
