pipeline {
    agent any

    environment {
        ACR_NAME = "acrdocker1.azurecr.io" // Your ACR login server
        ACR_USER = "acrdocker1"           // Your ACR username
        ACR_PASS = "8lx+S4w1dUtxFgNLHrC1hL6/8OgYaHy7P1sFpqxnk9+ACRBpi+q4" // Your ACR password
    }

    stages {
        stage('Checkout') {
            steps {
                deleteDir()
                git branch: 'main', url: 'https://github.com/Vikhyat2510/devops.git'
            }
        }

        stage('Debug Backend') {
            steps {
                dir('backend') {
                    sh 'pwd'
                    sh 'ls -la'
                    sh 'cat package.json'
                }
            }
        }

        stage('Backend Install & Test') {
            steps {
                dir('backend') {
                    sh 'npm install'
                    sh 'npm test'
                }
            }
        }

        stage('Build & Push Docker - Backend') {
            steps {
                dir('backend') {
                    // Login to ACR
                    sh 'docker login $ACR_NAME -u $ACR_USER -p $ACR_PASS'

                    // Build Docker image
                    sh 'docker build -t backend:latest .'

                    // Tag image for ACR
                    sh 'docker tag backend:latest $ACR_NAME/backend:latest'

                    // Push to ACR
                    sh 'docker push $ACR_NAME/backend:latest'
                }
            }
        }

        stage('Build & Push Docker - Frontend') {
            steps {
                dir('frontend') {
                    sh 'docker login $ACR_NAME -u $ACR_USER -p $ACR_PASS'
                    sh 'docker build -t frontend:latest .'
                    sh 'docker tag frontend:latest $ACR_NAME/frontend:latest'
                    sh 'docker push $ACR_NAME/frontend:latest'
                }
            }
        }

        stage('Build & Push Docker - Database') {
            steps {
                dir('database') {
                    sh 'docker login $ACR_NAME -u $ACR_USER -p $ACR_PASS'
                    sh 'docker build -t database:latest .'
                    sh 'docker tag database:latest $ACR_NAME/database:latest'
                    sh 'docker push $ACR_NAME/database:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Next: Use kubectl apply -f k8s/ to deploy backend, frontend, database"
                // Example:
                // sh 'kubectl apply -f k8s/backend-deployment.yaml'
                // sh 'kubectl apply -f k8s/frontend-deployment.yaml'
                // sh 'kubectl apply -f k8s/database-deployment.yaml'
            }
        }
    }
}
