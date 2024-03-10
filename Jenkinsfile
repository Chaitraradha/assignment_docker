pipeline {
    agent {
        docker {
            image 'terraform:1.0'
        }
    }
        
    parameters {
        string(name: 'environment', defaultValue: 'terraform', description: 'Workspace/environment file to use for deployment')
        booleanParam(name: 'autoApprove', defaultValue: false, description: 'Automatically run apply after generating plan?')
        booleanParam(name: 'destroy', defaultValue: false, description: 'Destroy infrastructure instead of applying changes?')
    }

    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        AWS_DEFAULT_REGION    = "ap-south-1"
    }

    stages {
        stage('Checkout') {
            steps {
                sh 'rm -rf assignment_docker' 
                sh 'git clone "https://github.com/Chaitraradha/assignment_docker.git"'
            }
        }

        stage('Terraform Init') {
            steps {
                dir('assignment_docker/terraform') {
                    script {
                        sh 'terraform init'
                    }
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                dir('assignment_docker/terraform') {
                    script {
                        sh "terraform plan -input=false -out=tfplan"
                        sh 'terraform show -no-color tfplan > tfplan.txt'
                    }
                }
            }
        }
        
        stage('Approval') {
            when {
                not { equals expected: true, actual: params.autoApprove }
                not { equals expected: true, actual: params.destroy }
            }
            steps {
                script {
                    def plan = readFile 'assignment_docker/terraform/tfplan.txt'
                    input message: "Do you want to apply the plan?",
                          parameters: [text(name: 'Plan', description: 'Please review the plan', defaultValue: plan)]
                }
            }
        }
        
        stage('Apply') {
            when {
                not { equals expected: true, actual: params.destroy }
            }
            steps {
                dir('assignment_docker/terraform') {
                    // Use -auto-approve flag if autoApprove is true
                    sh "terraform apply -input=false -auto-approve=${params.autoApprove} tfplan"
                }
            }
        }
    }
}

