pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ragavanworld/sample-app.git'
            }
        }
        stage('Build') {
            steps {
                echo "Building the app..."
            }
        }
        stage('Test') {
            steps {
                echo "Running tests..."
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying the app..."
            }
        }
    }
}
