pipeline {
    agent {
        // Use Node.js 16 Docker image as the build agent
        docker {
            image 'node:16'
        }
    }
    environment {
        SNYK_TOKEN = credentials('SNYK_API_TOKEN')  // Use the token stored in Jenkins as a secret
    }

    stages {
        stage('Install Dependencies') {
            steps {
                // Install the required dependencies
                sh 'npm install --save'
                sh 'npm install -g snyk'  
                sh 'npm audit fix'
            }
        }
        
        stage('Check Available Scripts') {
            steps {
                // Print available npm scripts to debug
                echo 'checking avaliable scripts'
                sh 'npm run'
            }
        }
         stage('Scan for Vulnerabilities with Snyk') {
            steps {
               echo 'Testing...'
	        snykSecurity(
		  snykInstallation: 'synk-api-token',
		  snykTokenId: 'synk-api-token',
		  // place other parameters here
		)
                
                //sh 'snyk test --all-projects' // Scan for vulnerabilities in all projects
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

