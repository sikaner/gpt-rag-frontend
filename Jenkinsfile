pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                echo "Cleaning workspace..."
                deleteDir()
            }
        }

        stage('Clone Repository') {
            steps {
                echo "Cloning repository..."
                git branch: "${env.BRANCH_NAME}", url: 'https://github.com/sikaner/gpt-rag-frontend.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing dependencies..."
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
                echo "Verifying build directory..."
                sh 'ls -la frontend/build || echo "Build directory not found!"'
            }
        }

        stage('Archive Build Artifacts') {
            steps {
                echo "Archiving artifacts..."
                archiveArtifacts artifacts: '**/build/**', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed. Check the logs for details."
        }
    }
}
