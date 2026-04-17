pipeline {
    agent any

    tools {
        // This matches the "Name" you gave the NodeJS installation in Jenkins settings
        nodejs 'Node 25' 
    }

    environment {
        // Tells Cypress to run in a virtual frame buffer (required for headless Linux)
        DISPLAY = ':99'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install') {
            steps {
                echo "Installing dependencies..."
                // npm ci is cleaner for CI/CD
                sh 'npm install'
            }
        }

        stage('Cypress Tests') {
            steps {
                echo "Running Cypress tests..."
                // xvfb-run handles the 'headless' display requirement on Linux servers
                sh 'xvfb-run npx cypress run'
            }
        }
    }

    post {
        always {
            // Standard artifact archiving
            archiveArtifacts artifacts: 'cypress/screenshots/**/*.png, cypress/videos/**/*.mp4', allowEmptyArchive: true
            junit 'cypress/results/*.xml'
        }
    }
}
