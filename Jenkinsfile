pipeline {
    agent {docker { image 'terraform:1.0'}
          }
    
    stages {
        stage('Terraform') {
            steps {
                script {
                    docker.image('terraform_image:1.0').inside('-v /home/ubuntu/docker:/workspace') {
                        // Change to the workspace directory
                        sh 'cd /workspace && terraform init && terraform plan && terraform apply -auto-approve'
                    }
                }
            }
        }
    }
}

