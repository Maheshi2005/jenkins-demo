pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building project...'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Dummy test log file create
                sh 'echo "This is a test log" > test-results.log'
                // Archive artifacts (log file)
                archiveArtifacts artifacts: 'test-results.log', allowEmptyArchive: true
            }
            post {
                always {
                    emailext(
                        to: 'yourgmail@gmail.com',
                        subject: "Test Stage - ${currentBuild.currentResult}",
                        body: """<p>Test stage completed.</p>
                                 <p>Status: ${currentBuild.currentResult}</p>
                                 <p><a href="${env.BUILD_URL}">View Build</a></p>""",
                        attachLog: true,
                        compressLog: true,
                        attachmentsPattern: 'test-results.log'
                    )
                }
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running security scan...'
                // Dummy scan report file create
                sh 'echo "This is a dummy security scan report" > scan-report.log'
                // Archive artifacts (scan report)
                archiveArtifacts artifacts: 'scan-report.log', allowEmptyArchive: true
            }
            post {
                always {
                    emailext(
                        to: 'yourgmail@gmail.com',
                        subject: "Security Scan - ${currentBuild.currentResult}",
                        body: """<p>Security scan finished.</p>
                                 <p>Status: ${currentBuild.currentResult}</p>
                                 <p><a href="${env.BUILD_URL}">View Build</a></p>""",
                        attachLog: true,
                        compressLog: true,
                        attachmentsPattern: 'scan-report.log'
                    )
                }
            }
        }
    }
}
