pipeline {
    agent {
        // Use Node.js 16 Docker image as the build agent
        docker {
            image 'node:16'
            // Ensures that the Docker container has internet access
            args '-u root'
        }
    }

    stages {
        stage('Install Dependencies') {
            steps {
                // Install the required dependencies
                sh 'npm install --save'
                sh 'npm audit fix'
            }
        }
        
        stage('Scan for Vulnerabilities with Snyk') {
            steps {
              script {
                // Retrieve Snyk API token securely from Jenkins credentials
                withCredentials([string(credentialsId: 'SNYK_API_TOKEN', variable: 'SNYK_TOKEN')]) {
                    
                    // Install Snyk CLI if needed
                    sh 'npm install -g snyk'
                    
                    // Authenticate with Snyk using the token
                    sh 'snyk auth ${SNYK_TOKEN}'
                    
                    // Perform vulnerability scan will fail the build if critical vulnerabilities are found
                    sh 'snyk test --severity-threshold=high > snyk-report.txt'
                }
              }
            }
            post {
                success {
                    // Snyk found no critical vulnerabilities
                    echo 'No critical vulnerabilities found in dependencies.'
                }
                failure {
                    // Handle failure in case vulnerabilities are found or the scan fails
                    echo 'Vulnerabilities found or Snyk scan failed.'
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'snyk-report.txt', allowEmptyArchive: true
            // Always clean up the workspace after the pipeline finishes
            cleanWs()
        }
    }
}
