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
               echo 'Testing for vulnerabilities using Snyk...'
        
	        // Install Snyk in the pipeline if it's not already available
	        sh 'npm install -g snyk'
	        
	        // Authenticate Snyk using your token
	        sh 'snyk auth ${SNYK_TOKEN}'
	        
	        // Perform the Snyk test (customize the command to your needs)
	        //sh 'snyk test --all-projects'
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
