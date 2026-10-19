pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/PRATHAM-GUPTA-6411/vle7-devops-pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t vle7-dashboard:latest .'
            }
        }

        stage('Load Image into Minikube') {
            steps {
                sh 'minikube image load vle7-dashboard:latest'
                sh 'minikube image ls | grep vle7-dashboard'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
                sh 'kubectl rollout status deployment/vle7-dashboard --timeout=180s'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'kubectl get deployment vle7-dashboard'
                sh 'kubectl get pods -l app=vle7-dashboard'
                sh 'kubectl get service vle7-dashboard'
            }
        }
    }

    post {
        success {
            echo 'VLE-7 CI/CD pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check the stage logs for details.'
        }
    }
}
