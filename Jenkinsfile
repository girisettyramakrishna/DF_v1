pipeline {
    agent any

    environment {
        R_LIBS_USER = "${HOME}/R/x86_64-pc-linux-gnu-library/4.3"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'git@github.com:girisettyramakrishna/DF_v2.git', credentialsId: 'your-creds-id'
            }
        }

        stage('Install R Packages') {
            steps {
                sh '''
                    Rscript -e "packages <- c('shiny', 'rsconnect'); new.packages <- packages[!(packages %in% installed.packages())]; if(length(new.packages)) install.packages(new.packages, repos='https://cloud.r-project.org/')"
                '''
            }
        }

        stage('Test App') {
            steps {
                sh 'Rscript -e "shiny::runApp(\'.\', port=8080, launch.browser=FALSE)" & sleep 5 && curl -f http://localhost:8080 || exit 1'
            }
        }

        stage('Deploy to shinyapps.io') {
            steps {
                sh '''
                    Rscript -e "rsconnect::setAccountInfo(name='psmlabs', token='8FAB74BDBB46C9CA05D75B7102711773', secret='9a6AwJZn3QacJOlVCdzWv6Ptj34mOsuIRE3OuNDI')"
                    Rscript -e "rsconnect::deployApp(appDir = '.', appName = 'DF_v2')"
                '''
            }
        }
    }

    post {
        failure {
            echo 'Pipeline failed.'
        }
        success {
            echo 'Pipeline completed successfully.'
        }
    }
}

