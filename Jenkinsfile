pipeline {
    agent any

    environment {
        IMAGE_NAME = "nginx-demo"
        IMAGE_TAG  = "v1"
        K8S_NAMESPACE = "app-demo-ns"
        KIND_CLUSTER = "kind1"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Load Image into Kind') {
            steps {
                echo "Loading Docker image into Kind cluster..."
                sh "kind load docker-image ${IMAGE_NAME}:${IMAGE_TAG} --name ${KIND_CLUSTER}"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Applying ConfigMap, Deployment and Service..."
                sh "kubectl apply -f k8s/nginx-configmap.yaml -n ${K8S_NAMESPACE}"
                sh "kubectl apply -f k8s/nginx-deployment.yaml -n ${K8S_NAMESPACE}"
                sh "kubectl apply -f k8s/service.yaml -n ${K8S_NAMESPACE}"
            }
        }

        stage('Verify Deployment') {
            steps {
                echo "Checking Pods and Services in namespace ${K8S_NAMESPACE}..."
                sh "kubectl get pods,svc -n ${K8S_NAMESPACE}"
            }
        }
    }
}

