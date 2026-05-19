pipeline {
    agent any

    environment {
        PROJECT_NAME = 'rishi-3-tier-project'
    }

    stages {
        stage('1. Git Checkout') {
            steps {
                echo 'Pulling latest code from GitHub Repository...'
                checkout scm
            }
        }

        stage('2. Terraform Quality Check') {
            steps {
                dir('terraform') {
                    echo 'Initializing Terraform Modules...'
                    sh 'terraform init'
                    echo 'Validating Infrastructure Code...'
                    sh 'terraform validate'
                }
            }
        }

        stage('3. Docker Web App Deployment') {
            steps {
                echo 'Stopping older containers if any...'
                sh 'sudo docker compose down || true'
                echo 'Building fresh 3-Tier Docker Images...'
                sh 'sudo docker compose up -d --build'
            }
        }

        stage('4. Verification Status') {
            steps {
                echo 'Checking live container status...'
                sh 'sudo docker ps'
            }
        }
    }

    post {
        success {
            echo 'Boom! Rishi your End-to-End DevOps Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check stage logs for errors.'
        }
    }
}
