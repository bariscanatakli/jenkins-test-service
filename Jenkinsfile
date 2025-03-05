pipeline {
    agent any
    
    stages {
        stage('Checkout Service') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/bariscanatakli/jenkins-test-service.git']]])
            }
        }
        
        stage('Install npm') {
            steps {
                sh '''
                if ! command -v npm &> /dev/null
                then
                    echo "npm could not be found, installing..."
                    sudo apt update
                    sudo apt install -y nodejs npm
                else
                    echo "npm is already installed"
                fi
                '''
            }
        }
        
        stage('Backend Tests') {
            steps {
                dir('backend/nestjs-api') {
                    sh 'npm install'
                    sh 'npm run test'
                }
            }
        }
        
        stage('Build Service Image') {
            steps {
                sh 'docker-compose build service'
            }
        }
        
        stage('Deploy Service') {
            steps {
                sh 'docker-compose up -d service'
            }
        }
    }
}