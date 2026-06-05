pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        AWS_ACCOUNT_ID = '746486152802'

        FRONTEND_REPO = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/blinkit-frontend"
        BACKEND_REPO = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/blinkit-backend"

        IMAGE_TAG = "v${env.BUILD_ID}"
    }

    stages {
        stage('Clean Workspace') {
            steps {
                echo "Clean up workspace"
                cleanWs()
            }
        }

        stage('Checkout Code') {
            steps {
                echo "Checkout Code"
                checkout scm
            }
        }
        
        stage('Build Docker Images') {
            steps {
                script {
                    echo "Building frontend Docker Image"
                    sh "docker build -t ${FRONTEND_REPO}:${IMAGE_TAG} ./client"

                    echo "Building backend docker image"
                    sh "docker build -t ${BACKEND_REPO}:${IMAGE_TAG} ./server"
                }
            }
        }
        
        stage('Push to AWS ECR') {
            steps {
                script {
                    echo "Logging in to amazon ecr"
                    sh """
                    aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                    """
                    
                    echo "Pushing Frontend Image"
                    sh "docker push ${FRONTEND_REPO}:${IMAGE_TAG}"

                    echo "Pushing backend image"
                    sh "docker push ${BACKEND_REPO}:${IMAGE_TAG}"
                }
            }
        }
    }
}