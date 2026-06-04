pipeline {
    agent any

    stages {
        stage ('code checkout'){
            steps {
                echo 'pulling code from github'
                checkout scm
            }
        }
        stage ('Inject Production Variable' ){
            steps {
                echo 'injecting production variable'
                sh 'cp /opt/app-secrets/.env.backend ./server/.env'
                sh 'cp /opt/app-secrets/.env.frontend ./client/.env'
            }
        }
        stage ('building and orchestrate new containers'){
            steps{
                echo 'dismantling old containers building new one'
                sh 'docker-compose down'
                sh 'docker-compose build --no-cache'
                sh 'docker-compose up -d --build --remove-orphans'
            }
        }
        stage ('clean up the working environment'){
            steps{
                echo 'removing images to save space'
                sh 'docker image prune -f'
            }
        }
    }
    post {
        success {
            echo '🚀 Pipeline successful! Blinkit Clone is live on AWS EC2!'
        }
        failure {
            echo '❌ Pipeline failed. Review the console logs to diagnose.'
        }
    }
}