pipeline {
    agent any

    environment {
        IMAGE_NAME = 'ragavanworld/nginx-demo:v1' // Your Docker Hub repo
        K8S_DIR = 'k8s'                           // Path where your YAMLs are
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

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DOCKER_HUB_CREDENTIALS', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh "docker push ${IMAGE_NAME}"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Applying Kubernetes manifests..."
                sh "kubectl apply -f ${K8S_DIR}/"
                sh "kubectl set image deployment/nginx-demo nginx=${IMAGE_NAME} -n app-demo-ns"
            }
        }

        stage('Verify Deployment') {
            steps {
                echo "Checking pod status..."
                sh "kubectl get pods -n app-demo-ns"
            }
        }
    }
}

