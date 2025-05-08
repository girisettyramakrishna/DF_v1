pipeline {
    agent any
    
    environment {
        // Define environment variables here, like paths to R libraries if needed
        SHINY_SERVER_PATH = '/srv/shiny-server/demand_forecasting'
    }
    
    stages {
        
        // Checkout the code from the repository
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        // Install R dependencies (if needed) - adjust based on your project
        stage('Install Dependencies') {
            steps {
                sh '''
                    sudo apt-get update
                    sudo apt-get install -y r-base
                    Rscript -e "install.packages(c('forecast', 'shiny', 'shinydashboard'))"
                '''
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
