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
