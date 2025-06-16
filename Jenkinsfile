pipeline {
    environment {
        IMAGE = "${params.IMAGE}"
    }
    stages {
        stage('Connect Kubectl to EKS') {
            steps {
                sh 'aws eks update-kubeconfig --region us-east-1 --name eks_cluster'
            }
        }
        stage('Deploy to EKS') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }
    }
}
