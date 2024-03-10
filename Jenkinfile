pipeline {
    agent {
        docker {
            image 'terraform_image:1.0'
            args '-v /home/ubuntu/docker'
        }
    }
    
    stages {
        stage('Terraform') {
            steps {
                sh 'cd /home/ubuntu/docker  :// Change to the workspace directory
                sh 'terraform init'
                sh 'terraform plan'
                sh 'terraform apply -auto-approve'
            }
        }
    }
}
