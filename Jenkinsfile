pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/sikaner/gpt-rag-frontend.git'
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo "Cloning repository..."
                deleteDir() // Clean workspace before cloning
                git branch: "${env.BRANCH_NAME}", url: "${REPO_URL}"
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing frontend dependencies..."
                dir('frontend') {
                    sh 'npm install'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                echo "Building frontend..."
                dir('frontend') {
                    sh 'npm run build'
                }
            }
        }

        stage('Archive Build Artifacts') {
            steps {
                echo "Archiving build artifacts..."
                archiveArtifacts artifacts: 'frontend/build/**', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "Build completed successfully!"
        }
        failure {
            echo "Build failed! Check the logs."
        }
    }
}
