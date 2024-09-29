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
                // Installing the required dependencies
                def logMessage = "Installing dependencies...\n"
                    writeFile(file: 'build-log.txt', text: logMessage)
                sh 'npm install --save'
                sh 'npm audit fix'
                logMessage = "Dependencies installed.\n"
                    writeFile(file: 'build-log.txt', text: logMessage, append: true)
            }
        }
        
        stage('Scan for Vulnerabilities with Snyk') {
            steps {
              script {
                / Retrieve Snyk API token securely from Jenkins credentials
                    withCredentials([string(credentialsId: 'SNYK_API_TOKEN', variable: 'SNYK_TOKEN')]) {
                        // Install Snyk CLI if needed
                        sh 'npm install -g snyk'
                        
                        // Authenticate with Snyk using the token
                        sh "snyk auth ${SNYK_TOKEN}"
                        
                        // Perform vulnerability scan and save output
                        def snykOutput = sh(script: 'snyk test --severity-threshold=high', returnStdout: true)
                        writeFile(file: 'snyk-report.txt', text: snykOutput)

                        // Log the results
                        def snykLog = "Snyk scan completed.\n"
                        snykLog += "Snyk Scan Results:\n"
                        snykLog += snykOutput
                        writeFile(file: 'build-log.txt', text: snykLog, append: true)
                }
              }
            }
            post {
                success {
                    script {
                        writeFile(file: 'build-log.txt', text: "No critical vulnerabilities found in 	dependencies.\n", append: true)
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
