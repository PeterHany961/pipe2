pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Pulls the code from your GitHub Repository
                checkout scm
            }
        }
        
        stage('Terraform Init') {
            steps {
                // Initializes the working directory
                sh 'terraform init'
            }
        }
        
        stage('Workspace Select') {
            steps {
                // Selects the environment workspace (dev, stg, prod)
                sh 'terraform workspace select dev || terraform workspace new dev'
            }
        }
        
        stage('Plan') {
            steps {
                // Generates the execution plan
                sh 'terraform plan -out=tfplan'
            }
        }
        
        stage('Approval') {
            steps {
                // Pauses pipeline for manual Approve/Abort intervention
                input message: 'Approve Terraform Apply?', ok: 'Approve'
            }
        }
        
        stage('Apply') {
            steps {
                // Executes the plan against the AWS Simulation Env
                sh 'terraform apply -input=false tfplan'
            }
        }
    }
    
    post {
        success {
            // Sends mail on success
            mail to: 'your-email@example.com',
                 subject: 'Pipeline Success',
                 body: 'Terraform apply completed successfully.'
        }
        failure {
            // Sends mail on fail
            mail to: 'your-email@example.com',
                 subject: 'Pipeline Failed',
                 body: 'Terraform pipeline failed.'
        }
    }
}
