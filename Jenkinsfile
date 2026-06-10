pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/shitesh372/project.git'
            }
        }
  stage('Deploy to AWS') {
    steps {
        withCredentials([sshUserPrivateKey(credentialsId: 'aws-key', keyFileVariable: 'SSH_KEY')]) {
            sh '''
              scp -o StrictHostKeyChecking=no -i $SSH_KEY index-aws.html ubuntu@100.56.101.120:/var/www/html/index.html
            '''
        }
    }
}


        stage('Deploy to Azure') {
            steps {
                sh '''
                    sshpass -p "AzureVM@Secure1234" scp index-azure.html azureuser@20.85.235.177:/var/www/html/index.html
                    sshpass -p "AzureVM@Secure1234" ssh azureuser@20.85.235.177 "sudo systemctl restart nginx"
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    echo "Checking AWS..."
                    curl -s http://100.56.101.120 | grep "Welcome to AWS"

                    echo "Checking Azure..."
                    curl -s http://20.85.235.177 | grep "Welcome to Azure"
                '''
            }
        }
    }
}
