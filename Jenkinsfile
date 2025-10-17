pipeline {
    agent any

    environment {
        IMAGE_NAME = "nginx-demo:v1"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Load Image into Kind') {
            steps {
                echo "Loading image into kind cluster..."
                sh "kind load docker-image ${IMAGE_NAME} --name kind1"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Deploying app to Kubernetes..."
                sh "kubectl apply -f k8s/nginx-configmap.yaml"
                sh "kubectl apply -f k8s/nginx-deployment.yaml"
                sh "kubectl apply -f k8s/service.yaml"
            }
        }

        stage('Verify Deployment') {
            steps {
                echo "Verifying deployment..."
                sh "kubectl get pods -n app-demo-ns"
            }
        }
    }
}

