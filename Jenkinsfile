pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // Ensure fresh install for the vulnerable app
                sh 'npm install --no-audit'
            }
        }

        stage('Run Tests') {
            steps {
                // Vulnerable apps often have failing tests; || true ensures the pipeline continues
                sh 'npm test || true'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                echo "Running security audit on nodejs-goof..."
                // This command will output all the CVEs for your video
                sh 'npm audit || true'
            }
        }
    }
}