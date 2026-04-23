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
                    url: 'https://github.com/YOUR_GITHUB_USERNAME/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests & Coverage') {
            steps {
                // We use || true to ensure the pipeline continues even if the 
                // intentionally "vulnerable" tests fail.
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit') {
            steps {
                sh 'npm audit || true'
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

                    # 3. Execute Analysis
                    # We force the branch name to 'main' to comply with SonarCloud Free Tier
                    ./sonar-scanner-cli/bin/sonar-scanner \
                      -Dsonar.token=${SONAR_TOKEN} \
                      -Dsonar.branch.name=main
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