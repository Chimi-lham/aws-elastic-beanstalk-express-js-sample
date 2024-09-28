pipeline {
    agent {
        // Use Node.js 16 Docker image as the build agent
        docker {
            image 'node:16'
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
            withCredentials([string(credentialsId: 'snyk-api-token', variable: 'SNYK_TOKEN')]) {
                
                // Install Snyk CLI if needed
                sh 'npm install -g snyk'
                
                // Authenticate with Snyk using the token
                sh 'snyk auth ${SNYK_TOKEN}'
                
                // Perform vulnerability scan
                sh 'snyk test --all-projects'
            }
            }
            post {
                success {
                    // Snyk found no critical vulnerabilities
                    echo 'No critical vulnerabilities found in dependencies.'
                }
                failure {
                    // Snyk found critical vulnerabilities
                    error 'Critical vulnerabilities detected! Halting the build.'
                }
            }
        }
           
        }

    post {
        always {
            // Always clean up the workspace after the pipeline finishes
            cleanWs()
        }
        
    }
}
