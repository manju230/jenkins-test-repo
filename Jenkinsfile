pipeline {

    agent any

    environment {

        AWS_REGION = 'ap-south-1'

    }

    stages {

        stage('Checkout') {

            steps {

                checkout scm

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

            }

        }

        stage('Approval') {

            when {

                branch 'main'

            }

            steps {

                input message: 'Approve Terraform Apply?', ok: 'Yes'

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
 
