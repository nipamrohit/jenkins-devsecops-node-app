pipeline {
    agent any

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
                sh 'npm test'
            }
        }

        stage('Static Code Analysis (ESLint)') {
            steps {
                sh 'npm run lint'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t node-app:latest .'
            }
        }

        stage('Trivy Security Scan') {
            steps {
                sh 'trivy image node-app:latest'
            }
        }

    }
}
