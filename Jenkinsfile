pipeline {
    agent any

    tools {
        nodejs 'sonarnode'  // Ensures NodeJS is installed on the agent
    }

    environment {
        NODEJS_HOME = 'C:\\Program Files\\nodejs'  // Replace with the configured Node.js tool path
        SONAR_SCANNER_PATH = 'C:\\Users\\lenovo\\Downloads\\sonar-scanner-cli-6.2.1.4610-windows-x64\\sonar-scanner-6.2.1.4610-windows-x64\\bin'
    }

    stages {
        stage('CheckOut') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm install
                '''
            }
        }

        stage('Lint') {
            steps {
                bat '''
                set PATH=%WORKSPACE%\\node_modules\\.bin;%PATH%
                npx lint
                '''
            }
        }

        stage('Build') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm run build
                '''
            }
        }

        stage('SonarAnalysis') {
            environment {
                SONAR_TOKEN = credentials('sonarQub-token') // SonarQube token stored in Jenkins credentials
            }
            steps {
                bat """
                set PATH=%NODEJS_HOME%;%PATH%
                sonar-scanner.bat -Dsonar.projectKey=MERN-Backend -Dsonar.sources=. -Dsonar.host.url=http://localhost:9000 -Dsonar.login=%SONAR_TOKEN%
                """
            }
        }
    }

    post {
        success {
            echo "Backend pipeline executed successfully!"
        }
        failure {
            echo "Backend pipeline failed. Please check the logs."
        }
    }
}
