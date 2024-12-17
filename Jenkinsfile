pipeline {
    agent any

    environment {
        EC2_USER = 'ubuntu'                        // Replace 'ubuntu' with the EC2 username
        EC2_IP = '54.167.160.183'              // Replace with your EC2 Public IP
        SSH_KEY = credentials('MyCredentials')     // Jenkins credential ID
        REPO_URL = 'https://github.com/sikaner/gpt-rag-frontend.git'
    }

    stages {
        stage('Block 1: Setup Frontend and Backend') {
            steps {
                echo "Deploying frontend and backend..."
                sshagent(['MyCredentials']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_IP} 'bash -s' << "ENDSSH"
                    echo "Cloning repository..."
                    rm -rf ~/gpt-rag-frontend
                    git clone ${REPO_URL} ~/gpt-rag-frontend

                    echo "Setting up Frontend..."
                    cd ~/gpt-rag-frontend/frontend
                    npm install
                    npm run build

                    echo "Setting up Backend..."
                    cd ~/gpt-rag-frontend/backend
                    python3 -m venv .env
                    source .env/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt || echo "No requirements.txt found"
                    deactivate

                    echo "Frontend and Backend setup complete."
                    ENDSSH
                    """
                }
            }
        }

        stage('Block 2: Frontend Only') {
            steps {
                echo "Deploying frontend only..."
                sshagent(['MyCredentials']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_IP} 'bash -s' << "ENDSSH"
                    echo "Cloning repository for frontend only..."
                    rm -rf ~/gpt-rag-frontend
                    git clone ${REPO_URL} ~/gpt-rag-frontend

                    echo "Setting up Frontend..."
                    cd ~/gpt-rag-frontend/frontend
                    npm install
                    npm run build

                    echo "Frontend setup complete."
                    ENDSSH
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Deployment to EC2 completed successfully!"
        }
        failure {
            echo "Deployment failed! Check Jenkins logs for errors."
        }
    }
}
