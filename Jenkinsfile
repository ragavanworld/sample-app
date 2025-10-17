pipeline {
    agent any

    environment {
        IMAGE_NAME = "nginx-demo:v1"
        DEPLOYMENT = "nginx-demo"
        NAMESPACE = "app-demo-ns"
        KIND_CLUSTER = "kind1"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Load Image into Kind') {
            steps {
                echo "Loading Docker image into Kind cluster..."
                sh "kind load docker-image ${IMAGE_NAME} --name ${KIND_CLUSTER}"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Updating Deployment in Kubernetes..."
                sh "kubectl set image deployment/${DEPLOYMENT} nginx=${IMAGE_NAME} -n ${NAMESPACE}"
            }
        }

        stage('Verify Deployment') {
            steps {
                echo "Checking pods status..."
                sh "kubectl get pods -n ${NAMESPACE}"
            }
        }
    }
}

