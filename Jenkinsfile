pipeline {
    agent any
    
    environment {
        SHINY_SERVER_PATH = '/srv/shiny-server/demand_forecasting'
    }
    
    stages {
        
        // Checkout the code from the repository
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        // Skip the 'Install Dependencies' stage if the necessary packages are already installed
        stage('Install Dependencies') {
            steps {
                script {
                    // You can check if dependencies are already installed, or just comment out if not needed
                    echo 'Skipping Install Dependencies stage as packages are already installed on the Jenkins server.'
                }
            }
        }

        // Deploy the app to Shiny Server
        stage('Deploy to Shiny Server') {
            steps {
                sh '''
                    # Copy the R app to Shiny Server directory
                    sudo cp -r * $SHINY_SERVER_PATH/
                    
                    # Set appropriate permissions for Shiny Server to access
                    sudo chown -R shiny:shiny $SHINY_SERVER_PATH
                    sudo systemctl restart shiny-server
                '''
            }
        }
        
    }
    
    post {
        success {
            echo 'Deployment Successful!'
        }
        
        failure {
            echo 'Deployment Failed!'
        }
    }
}
