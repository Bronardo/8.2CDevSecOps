pipeline {
    agent any

    environment {
        // Ensure this ID matches the one you created in Manage Jenkins > Credentials
        SONAR_TOKEN = credentials('SONAR_TOKEN')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'feature/sonarcloud-integration', 
                    url: 'https://github.com/Bronardo/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install --no-audit'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'  // Allows pipeline to continue despite test failures
            }
        }

        stage('Generate Coverage Report') {
            steps {
                // Ensure coverage report exists
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true' // This will show known CVEs in the output
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    sh '''
                    # 1. Install unzip if missing (required for the scanner)
                    if ! command -v unzip &> /dev/null; then
                        sudo apt-get update && sudo apt-get install -y unzip
                    fi

                    # 2. Download and Extract SonarScanner CLI if not already present
                    if [ ! -d "sonar-scanner-cli" ]; then
                        echo "Downloading SonarScanner..."
                        wget -q https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
                        unzip -o sonar-scanner-cli-5.0.1.3006-linux.zip
                        mv sonar-scanner-5.0.1.3006-linux sonar-scanner-cli
                    fi

                    # Check if the file exists before running (for debugging in console)
                    ls -l coverage/lcov.info || echo "LCOV report NOT found"

                    # Execute Analysis with explicit path fix
                    ./sonar-scanner-cli/bin/sonar-scanner \
                      -Dsonar.token=${SONAR_TOKEN} \
                      -Dsonar.branch.name=main \
                      -Dsonar.scm.disabled=true
                    '''
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline complete. Check SonarCloud dashboard for results."
        }
    }
}