
pipeline {
    agent any

    environment {
        DEPLOY_DIR = 'C:\\inetpub\\wwwroot\\Jenkins-CICD-Demo'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the web application'

                bat 'dir'

                bat '''
                if not exist index.html exit /b 1
                if not exist style.css exit /b 1
                if not exist script.js exit /b 1
                echo All project files found successfully
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Testing HTML file'

                bat '''
                powershell -NoProfile -Command "$html = Get-Content index.html -Raw; if ($html -match '<!DOCTYPE html>' -and $html -match '</html>') { Write-Host 'HTML validation passed' } else { Write-Error 'HTML validation failed'; exit 1 }"
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application to IIS'

                bat '''
                if not exist "%DEPLOY_DIR%" mkdir "%DEPLOY_DIR%"
                xcopy /E /I /Y index.html "%DEPLOY_DIR%\\"
                xcopy /E /I /Y style.css "%DEPLOY_DIR%\\"
                xcopy /E /I /Y script.js "%DEPLOY_DIR%\\"
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
