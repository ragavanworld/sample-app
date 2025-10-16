pipeline {
    agent any

    environment {
        IMAGE_NAME = "sample-app:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ragavanworld/sample-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh "/usr/local/bin/docker build -t \$IMAGE_NAME ."
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "Running Docker container..."
                sh "/usr/local/bin/docker stop sample-app || true"
                sh "/usr/local/bin/docker rm sample-app || true"
                sh "/usr/local/bin/docker run -d -p 8080:80 --name sample-app \$IMAGE_NAME"
            }
        }

        stage('Deploy to kind Cluster') {
            steps {
                echo "Deploying to kind Kubernetes cluster..."
                sh """
                cat <<EOF | /usr/local/bin/kubectl apply -f -
                apiVersion: v1
                kind: Pod
                metadata:
                  name: sample-app
                spec:
                  containers:
                  - name: sample-app
                    image: sample-app:latest
                    ports:
                    - containerPort: 80
                EOF
                """
            }
        }

        stage('Test') {
            steps {
                echo "Testing app availability..."
                sh "/usr/local/bin/curl http://localhost:8080"
            }
        }
    }
}

