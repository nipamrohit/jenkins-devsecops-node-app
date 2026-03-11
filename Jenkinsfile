pipeline {
ageny any
  
  stages {
	stage ('Checkout code'){
		step{
		checkout scm 
		}
	}
	stage ('Install Dependencies'){
		step{
		sh 'npm install'
		}
	}
        stage ('Run Test'){
                step{
                sh 'npm test'
                }
        }
        stage ('Build Docker Image'){
                step{
                sh 'docker build -t node-app:latest'
        	}        
	}
	stage('Static Code Analysis') { 
		steps { 
		sh 'npm run lint' 
		} 
	}
	stage('Trivy Security Scan') { 
		steps { 
		sh 'trivy image node-app:latest' 
		} 
	}
        
 	}	
}
