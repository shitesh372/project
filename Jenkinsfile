pipeline {
    agent any

    environment {
        AWS_HOST = "ubuntu@54.197.78.72"
        AZURE_HOST = "ubuntu@20.85.247.114"
    }

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/<your-username>/<your-repo>.git'
            }
        }

        stage('Deploy to AWS') {
            steps {
                sh """
                scp index-aws.html ${AWS_HOST}:/tmp/
                ssh ${AWS_HOST} '
                    sudo mv /tmp/index-aws.html /var/www/html/index.html &&
                    sudo systemctl restart nginx
                '
                """
            }
        }

        stage('Deploy to Azure') {
            steps {
                sh """
                scp index-azure.html ${AZURE_HOST}:/tmp/
                ssh ${AZURE_HOST} '
                    sudo mv /tmp/index-azure.html /var/www/html/index.html &&
                    sudo systemctl restart nginx
                '
                """
            }
        }
    }
}
