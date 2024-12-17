pipeline {
    agent any

    environment {
        // Define your EC2 instance's public IP and credentials ID
        EC2_IP = "54.167.160.183"
        SSH_CREDENTIALS_ID = "54.167.160.183"  // Name of your Jenkins SSH credentials
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/sikaner/gpt-rag-frontend.git'
            }
        }

        stage('Install Backend Dependencies') {
            steps {
                script {
                    sshagent([54.167.160.183]) {
                        sh """
                        ssh -o StrictHostKeyChecking=no ubuntu@${EC2_54.167.160.183} \
                        'cd ${BACKEND_DIR} && pip3 install -r requirements.txt'
                        """
                    }
                }
            }
        }

        stage('Install Frontend Dependencies') {
            steps {
                script {
                    sshagent([54.167.160.183]) {
                        sh """
                        ssh -o StrictHostKeyChecking=no ubuntu@${EC2_54.167.160.183} \
                        'cd ${FRONTEND_DIR} && npm install'
                        """
                    }
                }
            }
        }

        stage('Build Frontend') {
            steps {
                script {
                    sshagent([54.167.160.183]) {
                        sh """
                        ssh -o StrictHostKeyChecking=no ubuntu@${EC2_54.167.160.183} \
                        'cd ${FRONTEND_DIR} && npm run build'
                        """
                    }
                }
            }
        }

        stage('Start Backend') {
            steps {
                script {
                    sshagent([SSH_CREDENTIALS_ID]) {
                        sh """
                        ssh -o StrictHostKeyChecking=no ubuntu@${EC2_54.167.160.183} \
                        'cd ${BACKEND_DIR} && python3 app.py &'
                        """
                    }
                }
            }
        }

        stage('Start Frontend') {
            steps {
                script {
                    sshagent([SSH_CREDENTIALS_ID]) {
                        sh """
                        ssh -o StrictHostKeyChecking=no ubuntu@${EC2_54.167.160.183} \
                        'cd ${FRONTEND_DIR}/build && python3 -m http.server 80 &'
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
