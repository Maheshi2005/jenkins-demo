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
        script {
          if (isUnix()) {
            sh 'echo "This is a test log" > test-results.log'
          } else {
            bat 'echo This is a test log > test-results.log'
          }
        }
        archiveArtifacts artifacts: 'test-results.log', allowEmptyArchive: true
      }
      post {
        always {
          emailext(
            to: 'ayodyaekanayaka8@gmail.com',
            subject: "Test Stage - ${currentBuild.currentResult}",
            body: """<p>Test stage completed with status: <b>${currentBuild.currentResult}</b>.</p>
                     <p><a href="${env.BUILD_URL}">Open build</a></p>""",
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
        script {
          if (isUnix()) {
            sh 'echo "This is a dummy security scan report" > scan-report.log'
          } else {
            bat 'echo This is a dummy security scan report > scan-report.log'
          }
        }
        archiveArtifacts artifacts: 'scan-report.log', allowEmptyArchive: true
      }
      post {
        always {
          emailext(
            to: 'ayodyaekanayaka8@gmail.com',
            subject: "Security Scan - ${currentBuild.currentResult}",
            body: """<p>Security scan finished with status: <b>${currentBuild.currentResult}</b>.</p>
                     <p><a href="${env.BUILD_URL}">Open build</a></p>""",
            attachLog: true,
            compressLog: true,
            attachmentsPattern: 'scan-report.log'
          )
        }
      }
    }
  }
}
