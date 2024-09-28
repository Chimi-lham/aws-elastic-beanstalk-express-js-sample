pipeline {
    agent {
        // Use Node.js 16 Docker image as the build agent
        docker {
            image 'node:16'
        }
    }
    environment {
        SNYK_TOKEN = credentials('snyk-api-token') // Securely load Snyk API token from Jenkins credentials
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
         
        
    }

    post {
        always {
            // Always clean up the workspace after the pipeline finishes
            cleanWs()
        }
        
    }
}

