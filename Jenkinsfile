pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        stage('Check Kubernetes') {
            steps {
                bat 'kubectl get nodes'
            }
        }
        stage('Build Docker Image') {
            steps {
                bat 'minikube image build -t library-management:1.0 .'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f k8s.yaml'
            }
        }
        stage('Publish Static Copy') {
            steps {
                bat 'copy /Y index.html "%USERPROFILE%\\Desktop\\library-management-app.html"'
            }
        }
        stage('Verify Deployment') {
            steps {
                bat 'kubectl rollout status deployment/library-management'
                bat 'kubectl get pods'
                bat 'kubectl get svc'
            }
        }
    }

    post {
        success {
            echo 'Library Management application deployed successfully.'
        }
        failure {
            echo 'Deployment failed. Check the console output.'
        }
    }
}
