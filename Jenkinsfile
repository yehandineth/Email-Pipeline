pipeline {
  agent any
  options { timestamps() }

  triggers { pollSCM('H/5 * * * *') }
  
  environment {
    MAIL_TO = 'ydinethw@gmail.com'
    MAIL_FROM = 'headshot999plus@gmail.com
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
        echo 'Task: Run unit tests for code correctness and integration tests to verify components work together.'
        echo 'Tool: JUnit (unit), Maven Failsafe or Testcontainers (integration)'
      }
      post {
        success {
          emailext(
            to: env.MAIL_TO,
            subject: "Stage 2 (Tests): SUCCESS — ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Stage 2 finished SUCCESS.\nBuild: ${env.BUILD_URL}",
            attachLog: true
          )
        }
        unstable {
          emailext(
            to: env.MAIL_TO,
            subject: "Stage 2 (Tests): UNSTABLE — ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Stage 2 finished UNSTABLE.\nBuild: ${env.BUILD_URL}",
            attachLog: true
          )
        }
        failure {
          emailext(
            to: env.MAIL_TO,
            from: env.MAIL_FROM,
            subject: "Stage 2 (Tests): FAILURE — ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Stage 2 finished FAILURE.\nBuild: ${env.BUILD_URL}",
            attachLog: true
          )
        }
      }
    }

    stage('Stage 3: Code Analysis') {
      steps {
        echo 'Task: Perform static code analysis to check quality, standards, and detect issues.'
        echo 'Tool: SonarQube'
      }
    }

    stage('Stage 4: Security Scan') {
      steps {
        echo 'Task: Scan code and dependencies for known vulnerabilities.'
        echo 'Tool: Snyk'
      }
      post {
        success {
          emailext(
            to: env.MAIL_TO,
            subject: "Stage 4 (Security Scan): SUCCESS — ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Stage 4 finished SUCCESS.\nBuild: ${env.BUILD_URL}",
            attachLog: true
          )
        }
        unstable {
          emailext(
            to: env.MAIL_TO,
            from: env.MAIL_FROM,
            subject: "Stage 4 (Security Scan): UNSTABLE — ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Stage 4 finished UNSTABLE.\nBuild: ${env.BUILD_URL}",
            attachLog: true
          )
        }
        failure {
          emailext(
            to: env.MAIL_TO,
            subject: "Stage 4 (Security Scan): FAILURE — ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Stage 4 finished FAILURE.\nBuild: ${env.BUILD_URL}",
            attachLog: true
          )
        }
      }
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
