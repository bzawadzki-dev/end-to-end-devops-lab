pipeline {
    agent any
    stages {
        stage('Pobieranie kodu') {
            steps {
                checkout scm
            }
        }
        stage('Budowanie obrazu Docker') {
            steps {
                sh 'docker build -t moja-apka-devops:latest .'
            }
        }
        stage('Transfer obrazu do K3s') {
            steps {
                sh 'docker save moja-apka-devops:latest | sudo k3s ctr images import -'
            }
        }
        stage('Wdrożenie na Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl rollout restart deployment moja-apka-k8s'
            }
        }
    }
}
