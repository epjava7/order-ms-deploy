pipeline {
    environment {
        AWS_REGION = 'us-east-1'
        IMAGE = "${params.IMAGE}"
    }
    stages {
        stage('Terraform Init') {
            steps {
                withAWS(credentials: 'my-aws-creds', region: "${AWS_REGION}") {
                    sh 'cd terraform && terraform init'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                withAWS(credentials: 'my-aws-creds', region: "${AWS_REGION}") {
                    sh 'cd terraform && terraform apply -auto-approve'
                }
            }
        }

        stage('Connect kubectl to eks cluster') {
            steps {
                withAWS(credentials: 'my-aws-creds', region: "${AWS_REGION}") {
                    sh 'aws eks update-kubeconfig --region ${AWS_REGION} --name eks_cluster'
                }
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
                input message: 'Destroy?', ok: 'Yes'
                withAWS(credentials: 'my-aws-creds', region: "${AWS_REGION}") {
                    sh 'cd terraform && terraform destroy -auto-approve'
                }
            }
        }
    }
}
