pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
                // You can add build commands here, like 'mvn clean install' or 'npm install'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
                // Add test commands here, like 'mvn test' or 'npm test'
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying the project...'
                // Add deployment steps here, like copying files, or 'docker-compose up'
            }
        }
    }
    
    post {
        always {
            echo 'Cleaning up...'
            // Add any clean-up steps here
        }
    }
}

