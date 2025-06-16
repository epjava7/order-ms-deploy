pipeline {
    environment {
        IMAGE = "${params.IMAGE}"
    }
    stages {
        stage('Terraform Init') {
            steps {
                sh 'cd terraform && terraform init'
            }
        }

        stage('Terraform Apply') {
            steps {
                sh 'cd terraform && terraform apply -auto-approve'
            }
        }

        stage('Connect kubectl to eks cluster') {
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

        stage('Terraform Destroy') {
            steps {
                input message: 'Destory?', ok: 'Yes'
                sh 'cd terraform && terraform destroy -auto-approve'
            }
        }
    }
}
