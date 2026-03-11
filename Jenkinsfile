pipeline {
    agent any

    environment {
        IMAGE_NAME = "node-app"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('ESLint Analysis') {
            steps {
                sh 'npx eslint . -f checkstyle -o eslint-report.xml || true'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Trivy Scan') {
            steps {
                sh 'trivy image --format sarif -o trivy-report.sarif $IMAGE_NAME:$IMAGE_TAG'
            }
        }

        stage('Publish Reports') {
            steps {
                recordIssues tools: [
                    checkStyle(pattern: 'eslint-report.xml'),
                    sarif(pattern: 'trivy-report.sarif')
                ]
            }
        }

    }

    post {
        always {
            cleanWs()
        }
    }
}
