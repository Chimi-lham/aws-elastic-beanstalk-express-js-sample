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

        stage('Build Application') {
            steps {
                script{
                echo 'building project'
                sh 'CI=false npm run build'
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

