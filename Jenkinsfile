pipeline {
    agent any
    
    environment {
        SHINY_SERVER_PATH = '/srv/shiny-server/demand_forecasting'
    }
    
    stages {
        
        // Checkout the code from the Git repository
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        // Install Dependencies (skipping this stage as packages are already installed)
        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Skipping Install Dependencies stage as packages are already installed on the Jenkins server.'
                }
            }
        }

        // Deploy the app to Shiny Server
        stage('Deploy to Shiny Server') {
            steps {
                script {
                    // Debugging: List the contents of the workspace
                    echo 'Listing workspace contents:'
                    sh 'ls -alh'
                }
                
                sh '''
                    # Ensure the Shiny Server directory exists
                    sudo mkdir -p $SHINY_SERVER_PATH
                    
                    # Debugging: Check if files exist
                    echo "Checking if required files exist:"
                    ls -alh

                    # Copy the required files to the Shiny Server directory (use quotes for file names with spaces)
                    sudo cp -r "Jenkinsfile" "Latest" "New folder" "Product_data.csv" "_Rhistory" "app.R" "b.csv" "bivariate.R" "data" "data.R" "deploy.R" "error.csv" "forecast" "forecast.R" "multi.csv" "myoutputfile.html" "output.R" "packages_to_be_installed.R" "uni_error.csv" "univariate.R" "www" $SHINY_SERVER_PATH/

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
