pipeline {
    agent {
        // Use Node.js 16 Docker image as the build agent
        docker {
            image 'node:16'
        }
    }
    environment {
        SNYK_TOKEN = credentials('SNYK_API_TOKEN') // Securely load Snyk API token from Jenkins credentials
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
                script {
                    echo 'Running Snyk vulnerability test...'
                    // Run Snyk test with the Snyk CLI in Docker
                    sh '''
                        docker run --rm \
                        -e SNYK_TOKEN=${SNYK_TOKEN} \
                        -v $(pwd):/project \
                        snyk/snyk:latest test --all-projects
                    '''
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
            node('') {
                // Always clean up the workspace after the pipeline finishes
                cleanWs()
            }
        }
    }
}

