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
        
        stage('Check Available Scripts') {
            steps {
                // Print available npm scripts to debug
                echo 'checking avaliable scripts'
                sh 'npm run'
            }
        }
         stage('Scan for Vulnerabilities with Snyk') {
            steps {
                // Run Snyk to scan for vulnerabilities
                sh 'snyk test --all-projects' // Scan for vulnerabilities in all projects
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

