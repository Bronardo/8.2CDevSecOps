pipeline {
    agent any
    
    // Triggers a build when a change is detected via polling
    triggers {
        pollSCM('H/1 * * * *')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Description: Compiling the source code and packaging into a deployable artifact.'
                echo 'Tool: Maven / npm'
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                echo 'Description: Running unit tests for logic validation and integration tests for component connectivity.'
                echo 'Tool: JUnit / Jest'
            }
        }
        stage('Code Analysis') {
            steps {
                echo 'Description: Static code analysis to identify code smells and technical debt.'
                echo 'Tool: SonarCloud'
            }
        }
        stage('Security Scan') {
            steps {
                echo 'Description: Scanning application dependencies for known vulnerabilities.'
                echo 'Tool: npm audit / Snyk'
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo 'Description: Deploying the artifact to a staging environment for final validation.'
                echo 'Tool: kubectl (Staging Namespace) / AWS EC2'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo 'Description: Running end-to-end tests in a production-like environment.'
                echo 'Tool: Postman / Selenium'
            }
        }
        stage('Deploy to Production') {
            steps {
                echo 'Description: Final rollout of the application to the production server.'
                echo 'Tool: kubectl rollout / AWS EC2'
            }
        }
    }
}