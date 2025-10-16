pipeline {
    agent any

    environment {
        KUBECONFIG = '/root/.kube/config'  // adjust if Jenkins runs as another user
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/ragavanworld/sample-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh 'docker build -t sample-nginx:latest . || echo "Skipping Docker build (using nginx:latest)"'
            }
        }

        stage('Deploy to Kind Cluster') {
            steps {
                echo "Deploying to kind cluster..."
                sh '''
                    kubectl apply -f k8s/nginx-deployment.yaml
                    echo "Deployment status:"
                    kubectl get pods -n app-demo-ns
                    kubectl get svc -n app-demo-ns
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                echo "Verifying Nginx pod..."
                sh '''
                    kubectl wait --for=condition=available --timeout=60s deployment/app-demo-pod -n app-demo-ns
                    echo "Access your app at: http://localhost:30909"
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful — Visit: http://localhost:30909"
        }
        failure {
            echo "❌ Deployment failed. Check Jenkins logs."
        }
    }
}

