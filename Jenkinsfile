pipeline {
  agent any

  stages {
    stage('Build') {
      steps { echo 'Building project...' }
    }

    stage('Test') {
      steps {
        echo 'Running tests...'
        script {
          if (isUnix()) { sh 'echo "This is a test log" > test-results.log' }
          else          { bat 'echo This is a test log > test-results.log' }
        }
        archiveArtifacts artifacts: 'test-results.log', allowEmptyArchive: true
      }
      post {
        always {
          emailext(
            to: '${DEFAULT_RECIPIENTS}',
            subject: "Test Stage - ${currentBuild.currentResult}",
            mimeType: 'text/html',
            body: """<p>Test stage completed: <b>${currentBuild.currentResult}</b></p>
                     <p><a href="${env.BUILD_URL}">Open build</a></p>""",
            attachmentsPattern: 'test-results.log',
            attachLog: true, compressLog: true
          )
        }
      }
    }

    stage('Security Scan') {
      steps {
        echo 'Running security scan...'
        script {
          if (isUnix()) { sh 'echo "This is a dummy security scan report" > scan-report.log' }
          else          { bat 'echo This is a dummy security scan report > scan-report.log' }
        }
        archiveArtifacts artifacts: 'scan-report.log', allowEmptyArchive: true
      }
      post {
        always {
          emailext(
            to: '${DEFAULT_RECIPIENTS}',
            subject: "Security Scan - ${currentBuild.currentResult}",
            mimeType: 'text/html',
            body: """<p>Security scan finished: <b>${currentBuild.currentResult}</b></p>
                     <p><a href="${env.BUILD_URL}">Open build</a></p>""",
            attachmentsPattern: 'scan-report.log',
            attachLog: true, compressLog: true
          )
        }
      }
    }
  }
}
