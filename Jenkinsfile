pipeline {
    agent any

    stages {
        stage('1. Git Checkout') {
            steps {
                echo 'Pulling latest code from GitHub Repository...'
                checkout scm
            }
        }

        stage('2. SonarQube Code Analysis') {
            steps {
                echo 'Running Local SonarQube Code Scan...'
                sh 'docker run --rm --network="host" -v "$(pwd):/usr/src" sonarsource/sonar-scanner-cli || true'
            }
        }

        stage('3. Terraform Quality Check') {
            steps {
                dir('terraform') {
                    echo 'Initializing Terraform Modules...'
                    sh 'terraform init'
                    echo 'Validating Infrastructure Code...'
                    sh 'terraform validate'
                }
            }
        }

        stage('4. Docker Web App Deployment') {
            steps {
                echo 'Stopping and cleaning any existing containers...'
                sh 'sudo docker compose down --remove-orphans || true'
                echo 'Building fresh 3-Tier Docker Images...'
                sh 'sudo docker compose up -d --build'
            }
        }
    }

    post {
        success {
            echo 'Boom! Rishi your End-to-End DevOps Pipeline with SonarQube executed successfully!'
        }
    }
}
