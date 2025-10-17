pipeline {
    agent any

    environment {
        APP_NAME = 'nginx-demo'
        DOCKER_IMAGE = 'nginx-demo:v1'
        K8S_DIR = 'k8s'
        K8S_CONTEXT = 'kind-kind1'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh """
                        kubectl config use-context ${K8S_CONTEXT}
                        kubectl apply -f ${K8S_DIR}/nginx-configmap.yaml
                        kubectl apply -f ${K8S_DIR}/nginx-deployment.yaml
                        kubectl apply -f ${K8S_DIR}/service.yaml
                    """
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    sh 'kubectl get pods -o wide'
                    sh 'kubectl get svc'
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful!"
        }
        failure {
            echo "❌ Deployment failed. Check Jenkins logs."
        }
    }
}
