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

        stage('Static Code Analysis (ESLint)') {
            steps {
                sh 'npm run lint > eslint-report.txt || true'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Trivy Security Scan') {
            steps {
                sh 'trivy image --format table -o trivy-report.txt $IMAGE_NAME:$IMAGE_TAG'
            }
        }

        stage('Archive Reports') {
            steps {
                archiveArtifacts artifacts: '*.txt', fingerprint: true
            }
        }

    }

    post {
        always {
            cleanWs()
        }
    }
}
