pipeline {
    agent any
    stages {
        stage('Deploy to K8s') {
            steps {
                echo "Applying ConfigMap and Deployment..."
                sh 'kubectl apply -f k8s/nginx-configmap.yaml'
                sh 'kubectl apply -f k8s/nginx-deployment.yaml'
                sh 'kubectl get pods,svc -n web-demo'
            }
        }
    }
}
