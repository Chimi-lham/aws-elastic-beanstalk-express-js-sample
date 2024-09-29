pipeline {
    agent {
        // Node.js 16 Docker image as the build agent
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
                    // Append the results to the build log
                    sh 'echo "Audit fix completed." >> build-log.txt'
                    sh 'echo "Starting Snyk vulnerability scan..." >> build-log.txt'
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
                    
                    // Perform vulnerability scan and capture output
                    def snykOutput = sh(script: 'snyk test --severity-threshold=high', returnStdout: true)
                   // Append the results to the build log
                    sh 'echo "Snyk scan completed." >> build-log.txt'
                    sh "echo 'Snyk Scan Results:\n${snykOutput}' >> build-log.txt"
                    }
                }
            }
            post {
                success {
                    script {
                        sh 'echo "No critical vulnerabilities found in dependencies." >> build-log.txt'
                    }
                }
                failure {
                    script {
                         sh 'echo "Vulnerabilities found or Snyk scan failed. Review the Snyk report for details." >> build-log.txt'
                    }
                }
            }
            stage('Synk Monitor'){
            steps{
              script{
              //monitor the project for security issues
               sh 'synk monitor'
              }}
            }
        }
    }

    post {
        always {
            // Archive the log and Snyk report
            archiveArtifacts artifacts: 'build-log.txt', allowEmptyArchive: true           
            // Clean up the workspace after the pipeline finishes
            cleanWs()
        }
    }
}

