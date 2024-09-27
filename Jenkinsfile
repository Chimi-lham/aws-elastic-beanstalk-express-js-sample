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

        stage('Install Snyk') {
            steps {
                // Install Snyk globally using npm
                sh 'npm install -g snyk'

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

