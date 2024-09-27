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

        stage('Start Application') {
            steps {
                // Start the application using the npm start command
                echo 'Starting the application...'
                sh 'npm start'
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

