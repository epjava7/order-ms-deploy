pipeline {
    agent any 
    environment {
        AWS_REGION = 'us-east-1'
        IMAGE = "${params.IMAGE}"
    }
    stages {
        stage('Terraform Init') {
            steps {
                withAWS(credentials: 'aws-creds', region: "${AWS_REGION}") {
                    sh 'cd terraform && terraform init'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                withAWS(credentials: 'aws-creds', region: "${AWS_REGION}") {
                    sh 'cd terraform && terraform apply -auto-approve'
                }
            }
        }

        stage('Connect kubectl to eks cluster') {
            steps {
                withAWS(credentials: 'aws-creds', region: "${AWS_REGION}") {
                    sh 'aws eks update-kubeconfig --region ${AWS_REGION} --name eks_cluster'
                }
            }
        }

        // need aws creds here bc kubectl needs auth with EKS
        stage('Deploy to EKS') {
            steps {
                withAWS(credentials: 'aws-creds', region: "${AWS_REGION}") {
                    sh 'kubectl apply -f k8s/deployment.yaml'
                    sh 'kubectl apply -f k8s/service.yaml'
                }
            }
        }

        stage('Terraform Destroy') {
            steps {
                input message: 'Destroy?', ok: 'Yes'
                withAWS(credentials: 'aws-creds', region: "${AWS_REGION}") {
                    sh 'cd terraform && terraform destroy -auto-approve'
                }
            }
        }
    }
}
