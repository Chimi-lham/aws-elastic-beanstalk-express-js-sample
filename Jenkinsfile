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
                script {
                                        
                    sh 'echo "Starting dependency installation..." > build-log.txt'

                    // Install the required dependencies
                    sh 'npm install --save'
                    sh 'echo "Dependencies installed successfully." >> build-log.txt'

                    // Fix any audit issues
                    sh 'npm audit fix'
                    sh 'echo "Audit fix completed." >> build-log.txt'

                    // Log that Snyk will be used next
                    sh 'echo "Starting Snyk vulnerability scan..." >> build-log.txt
                }
            }
        }
        
        stage('Scan for Vulnerabilities with Snyk') {
            steps {
                script {
                    // Retrieve Snyk API token securely from Jenkins credentials
                    withCredentials([string(credentialsId: 'SNYK_API_TOKEN', variable: 'SNYK_TOKEN')]) {
  
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
                    script {
                        writeFile(file: 'build-log.txt', text: "No critical vulnerabilities found in dependencies.\n", append: true)
                    }
                }
                failure {
                    script {
                        writeFile(file: 'build-log.txt', text: "Vulnerabilities found or Snyk scan failed. Review the Snyk report for details.\n", append: true)
                    }
                }
            }
        }
    }

    post {
        always {
            // Archive the log and Snyk report
            archiveArtifacts artifacts: 'build-log.txt', allowEmptyArchive: true
            archiveArtifacts artifacts: 'snyk-report.txt', allowEmptyArchive: true
            
            // Clean up the workspace after the pipeline finishes
            cleanWs()
        }
    }
}

