pipeline {
    agent any
    
    environment {
        // Define the Shiny Server deployment path
        SHINY_SERVER_PATH = '/srv/shiny-server/demand_forecasting'
    }
    
    stages {
        
        // Checkout the code from the Git repository
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        // Install R dependencies (if required)
        stage('Install Dependencies') {
            steps {
                script {
                    // You can skip this step if dependencies are already installed on the server.
                    echo 'Skipping Install Dependencies stage as packages are already installed on the Jenkins server.'
                }
            }
        }

        // Deploy the app to Shiny Server
        stage('Deploy to Shiny Server') {
            steps {
                sh '''
                    # Ensure the Shiny Server directory exists
                    sudo mkdir -p $SHINY_SERVER_PATH
                    
                    # Copy the required files to the Shiny Server directory
                    sudo cp -r Jenkinsfile Latest New folder Product_data.csv _Rhistory app.R b.csv bivariate.R data data.R deploy.R error.csv forecast forecast.R multi.csv myoutputfile.html output.R packages_to_be_installed.R uni_error.csv univariate.R www $SHINY_SERVER_PATH/
                    
                    # Set appropriate permissions for Shiny Server to access
                    sudo chown -R shiny:shiny $SHINY_SERVER_PATH
                    
                    # Restart Shiny Server to apply changes
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
