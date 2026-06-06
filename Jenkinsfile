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

        stage('Update Kubernetes Manifests') {
            steps {
                withCredentials([string(credentialsId: 'jenkins-gitops-token', variable: 'GITHUB_TOKEN')]) {
                    script {
                        echo "Cloning Infrastructure Repository..."
                        sh "git clone https://${GITHUB_TOKEN}@github.com/HardikYadav99/blinkit-eks-infra.git"
                        
                        dir('blinkit-eks-infra') {
                            echo "Dynamically updating image tags to ${IMAGE_TAG}..."
                            
                            // Using standard Linux sed to find any matching image string and swap it out
                            sh "sed -i 's|image: .*/blinkit-frontend:.*|image: ${FRONTEND_REPO}:${IMAGE_TAG}|g' k8s/frontend.yaml"
                            sh "sed -i 's|image: .*/blinkit-backend:.*|image: ${BACKEND_REPO}:${IMAGE_TAG}|g' k8s/backend.yaml"
                            
                            echo "Committing and pushing updated manifests to GitOps Repo..."
                            sh "git config user.email 'jenkins@hardik-devops.com'"
                            sh "git config user.name 'Jenkins CI'"
                            sh "git add k8s/frontend.yaml k8s/backend.yaml"
                            
                            // [skip ci] prevents an infinite loop if your infra repo triggers other builds
                            sh "git commit -m 'automated: update image tags to ${IMAGE_TAG} [skip ci]'"
                            sh "git push origin main"
                        }
                    }
                }
            }
        }
    }
}