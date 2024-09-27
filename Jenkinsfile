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

        stage('Build Application') {
            steps {
                // Build the application
                sh 'npm run build'
            }
        }
    }

    post {
        always {
            // Always clean up the workspace after the pipeline finishes
            cleanWs()
        }

        success {
            // Notify success (optional: integrate notifications like Slack or email)
            echo 'Build and deployment successful!'
        }

        failure {
            // Notify failure (optional: integrate notifications here as well)
            echo 'Build or deployment failed!'
        }
    }
}

