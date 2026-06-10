pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myapp:${BUILD_NUMBER} .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop myapp || true
                docker rm myapp || true
                docker run -d -p 5000:5000 --name myapp myapp:${BUILD_NUMBER}
                sleep 5
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh 'curl -f http://localhost:5000/health'
            }
        }
    }
}