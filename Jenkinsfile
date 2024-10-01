pipeline {
    agent any

    environment {
        EMAIL_RECIPIENT = 'arorasanya.2401@gmail.com'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
                // Specify the build automation tool e.g., Maven
                sh 'mvn clean install'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
                // Add the appropriate test automation tool e.g., JUnit, Selenium
                sh 'mvn test'
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Performing code analysis...'
                // Use a code analysis tool e.g., SonarQube
                sh 'sonar-scanner'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running security scan...'
                // Use a security scan tool e.g., OWASP Dependency-Check
                sh 'dependency-check.sh --project project-name --scan .'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging environment...'
                // Use appropriate deployment commands (e.g., to AWS EC2)
                sh 'scp target/*.jar user@staging-server:/path/to/deploy'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                // Use test automation tool on the staging server
                sh 'ssh user@staging-server "mvn integration-test"'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production...'
                // Use appropriate deployment commands for production
                sh 'scp target/*.jar user@production-server:/path/to/deploy'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace...'
            cleanWs()
        }
        success {
            mail to: "${EMAIL_RECIPIENT}",
                 subject: "Pipeline Succeeded: ${env.JOB_NAME} Build #${env.BUILD_NUMBER}",
                 body: "Good news! The pipeline has completed successfully.",
                 attachLog: true
        }
        failure {
            mail to: "${EMAIL_RECIPIENT}",
                 subject: "Pipeline Failed: ${env.JOB_NAME} Build #${env.BUILD_NUMBER}",
                 body: "Unfortunately, the pipeline has failed. Please check the logs.",
                 attachLog: true
        }
    }
}
