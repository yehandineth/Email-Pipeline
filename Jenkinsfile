pipeline {
  agent any
  options { timestamps() }

  environment {
    MAIL_TO = 'ydinethw@gmail.com'
  }

  stages {
    stage('Stage 1: Build') {
      steps {
        echo 'Task: Compile and package the source code.'
        echo 'Tool: Maven'
      }
    }

    stage('Stage 2: Unit and Integration Tests') {
      steps {
        script {
          echo 'Task: Run unit tests for code correctness and integration tests to verify components work together.'
          echo 'Tool: JUnit (unit), Maven Failsafe or Testcontainers (integration)'

          def statusText = 'SUCCESS' // theoretical stage
          writeFile file: 'stage2-tests.log', text: """[Stage] Stage 2: Unit and Integration Tests
[Status] ${statusText}
[Details] Printed Task/Tool as required."""
          def bodyTxt = readFile 'stage2-tests.log'
          mail to: env.MAIL_TO,
               subject: "Stage 2 (Tests): ${statusText} — ${env.JOB_NAME} #${env.BUILD_NUMBER}",
               body: bodyTxt
        }
      }
      post { always { archiveArtifacts artifacts: 'stage2-tests.log', allowEmptyArchive: true } }
    }

    stage('Stage 3: Code Analysis') {
      steps {
        echo 'Task: Perform static code analysis to check quality, standards, and detect issues.'
        echo 'Tool: SonarQube'
      }
    }

    stage('Stage 4: Security Scan') {
      steps {
        script {
          echo 'Task: Scan code and dependencies for known vulnerabilities.'
          echo 'Tool: Snyk'

          def statusText = 'SUCCESS' // theoretical stage
          writeFile file: 'stage4-audit.log', text: """[Stage] Stage 4: Security Scan
[Status] ${statusText}
[Details] Printed Task/Tool as required."""
          def bodyTxt = readFile 'stage4-audit.log'
          mail to: env.MAIL_TO,
               subject: "Stage 4 (Security Scan): ${statusText} — ${env.JOB_NAME} #${env.BUILD_NUMBER}",
               body: bodyTxt
        }
      }
      post { always { archiveArtifacts artifacts: 'stage4-audit.log', allowEmptyArchive: true } }
    }

    stage('Stage 5: Deploy to Staging') {
      steps {
        echo 'Task: Deploy the packaged application to a staging server (e.g., AWS EC2).'
        echo 'Tool: Ansible'
      }
    }

    stage('Stage 6: Integration Tests on Staging') {
      steps {
        echo 'Task: Execute end-to-end/integration tests in a production-like environment.'
        echo 'Tool: Postman/Newman'
      }
    }

    stage('Stage 7: Deploy to Production') {
      steps {
        echo 'Task: Deploy the application to the production environment.'
        echo 'Tool: Ansible'
      }
    }
  }
}
