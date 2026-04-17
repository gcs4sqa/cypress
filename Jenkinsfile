pipeline {
    agent {
        docker {
            // Using the official Cypress 'factory' image that includes 
            // Chrome, Firefox, and all system dependencies
            image 'cypress/browsers:node-20.11.0-chrome-121.0.6167.85-ff-122.0-edge-121.0.2277.83-1'
            args '--shm-size=2g' // Prevents browser crashes in Docker
        }
    }

    environment {
        // Example: If your app needs an API key or base URL
        CYPRESS_BASE_URL = 'https://staging.your-app.com'
    }

    stages {
        stage('Checkout') {
            steps {
                // Pulls the code from the GitHub repo defined in the job
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // npm ci is faster and more reliable for CI environments
                sh 'npm ci'
            }
        }

        stage('Cypress Tests') {
            steps {
                // Run tests using Chrome in headless mode
                // '|| true' ensures the pipeline continues to the post-stage 
                // if tests fail, so we can grab the screenshots/videos
                sh 'npx cypress run --browser chrome'
            }
        }
    }

    post {
        always {
            // This captures the test evidence regardless of success or failure
            archiveArtifacts artifacts: 'cypress/screenshots/**/*.png, cypress/videos/**/*.mp4', allowEmptyArchive: true
            
            // If you use the JUnit reporter, you can visualize results in Jenkins
            junit 'cypress/results/*.xml' 
        }
        
        failure {
            echo "Tests failed! Check the archived screenshots/videos in the build summary."
        }
    }
}
