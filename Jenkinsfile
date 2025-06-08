pipeline {
    agent any
    environment {
        IMAGE = "${params.IMAGE}"
        AWS_ACCESS_KEY_ID = credentials('aws-access-key') 
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key') 
    }
    stages {
        stage('Configure Kubeconfig') {
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