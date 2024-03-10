
pipeline {
    agent {
        docker {
            image 'terraform_image:1.0'
            args '-v /home/ubuntu/docker:/workspace'  // Mounting the host directory to /workspace in the Docker container
        }
    }
    
    stages {
        stage('Terraform') {
            steps {
                script {
                    // Change to the workspace directory
                    sh 'cd /workspace'
                    sh 'terraform init'
                    sh 'terraform plan'
                    sh 'terraform apply -auto-approve'
                }
            }
        }
    }
}
