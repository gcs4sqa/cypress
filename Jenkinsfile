pipeline {
    agent any
    
    tools {
        nodejs 'Node 20'
    }

    environment {
        // This forces the heavy Cypress binary to live inside your project folder
        CYPRESS_RUN_BINARY = "${WORKSPACE}/cypress-binary/Cypress/Cypress"
        CYPRESS_CACHE_FOLDER = "${WORKSPACE}/cypress-cache"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install') {
            steps {
                echo "Installing dependencies and binary..."
                // This tells npm where to put the binary during install
                sh 'export CYPRESS_CACHE_FOLDER=$WORKSPACE/cypress-cache && npm install'
                
                // Verify the binary is actually there
                sh 'npx cypress verify'
            }
        }

        stage('Run Cypress') {
            steps {
                echo "Running tests..."
                // Ensure the test stage also knows where the cache is
                sh 'export CYPRESS_CACHE_FOLDER=$WORKSPACE/cypress-cache && xvfb-run npx cypress run'
            }
        }
    }
}
