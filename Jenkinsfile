pipeline {
    agent any
    
    stages {
        stage('Checkout Service') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/bariscanatakli/jenkins-test-service.git']]])
            }
        }
        
        stage('Install Dependencies') {
            steps {
                dir('backend/nestjs-api') {
                    sh 'npm install'
                }
            }
        }
        
        stage('Run Tests') {
            steps {
                dir('backend/nestjs-api') {
                    sh 'npm run test'
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh 'docker-compose build backend'
            }
        }
        
        stage('Deploy Service') {
            steps {
                sh 'docker-compose up -d backend'
            }
        }
    }
}