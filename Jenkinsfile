pipeline {
    agent any

    environment {
        APP_NAME = "demand_forecasting"
        DEPLOY_DIR = "/srv/shiny-server/${APP_NAME}"
        GIT_REPO = "https://github.com/girisettyramakrishna/DF_v1.git"
    }

    stages {
        stage('Checkout') {
            steps {
                git url: "${GIT_REPO}", branch: 'master'
            }
        }

        stage('Deploy App to Shiny Server') {
            steps {
                sh '''
                    echo "Cleaning old deployment..."
                    sudo rm -rf ${DEPLOY_DIR}

                    echo "Creating new deployment directory..."
                    sudo mkdir -p ${DEPLOY_DIR}

                    echo "Copying files to deployment directory..."
                    sudo cp -r * ${DEPLOY_DIR}/

                    echo "Changing ownership to shiny user..."
                    sudo chown -R shiny:shiny ${DEPLOY_DIR}
                '''
            }
        }

        stage('Restart Shiny Server') {
            steps {
                sh 'sudo systemctl restart shiny-server'
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful. App is available at: http://192.168.42.105:3838/${APP_NAME}/"
        }
        failure {
            echo "❌ Deployment failed. Please check the Jenkins logs for error details."
        }
    }
}
