pipeline {
    agent any
    environment {
        IMAGE = "${params.IMAGE}"
    }
    stages {
        stage('Configure Kubeconfig to connect kubectl to EKS') {
            steps {
                sh 'aws eks update-kubeconfig --region us-east-1 --name eks_cluster'
            }
        }
        stage('Update K8s Manifest') {
            steps {
                sh "sed -i 's|image: .*|image: $IMAGE|' k8s/deployment.yaml"
            }
        }
        stage('Deploy to EKS') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
    }
}