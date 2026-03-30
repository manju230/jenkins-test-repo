pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        TF_CLI_ARGS = "-no-color"   // Ensures clean, aligned Terraform output in Jenkins console
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/manju230/jenkins-test-repo.git', branch: "${env.BRANCH_NAME}"
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }

        stage('Terraform Plan') {
            when {
                branch 'develop'
            }
            steps {
                sh 'terraform plan -out=tfplan'
                archiveArtifacts artifacts: 'tfplan', fingerprint: true
            }
        }

        stage('Approval') {
            when {
                branch 'main'
            }
            steps {
                input message: 'Do you want to apply Terraform changes?', ok: 'Approve'
            }
        }

        stage('Terraform Apply') {
            when {
                branch 'main'
            }
            steps {
                sh 'terraform apply -auto-approve'
            }
        }
    }
}
