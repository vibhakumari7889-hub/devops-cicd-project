pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                sh '''
                docker run --rm \
                  -v "$PWD/app:/app" \
                  -w /app \
                  python:3.11-slim \
                  sh -c "pip install --no-cache-dir -r requirements.txt && pytest"
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-cicd-app:latest .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker rm -f devops-cicd-container || true
                docker run -d --name devops-cicd-container -p 5000:5000 devops-cicd-app:latest
                '''
            }
        }
    }
}
