pipeline {
    agent any
    
    tools {
        nodejs 'Node 25'
    }

    environment {
        // We define the cache location in the workspace
        CYPRESS_CACHE_FOLDER = "${WORKSPACE}/.cypress-cache"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // 1. Install the npm package
                sh 'npm install'
                
                // 2. Explicitly install the binary into our custom cache folder
                sh 'npx cypress install'
                
                // 3. Verify it's ready to go
                sh 'npx cypress verify'
            }
        }

        stage('Run Cypress') {
            steps {
                // xvfb-run provides the virtual display for the binary
                sh 'xvfb-run npx cypress run'
            }
        }
    }
    
    post {
        always {
            archiveArtifacts artifacts: 'cypress/screenshots/**/*.png, cypress/videos/**/*.mp4', allowEmptyArchive: true
        }
    }
}