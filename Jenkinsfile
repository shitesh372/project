pipeline {
    agent any

    environment {
        AWS_HOST = "ubuntu@54.197.78.72"
        AZURE_HOST = "azureuser@13.68.190.21"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/shitesh372/project.git'
            }
        }

        stage('Deploy with Ansible') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'aws-key-id', keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                        # Run Ansible using Jenkins-injected AWS key
                        ansible-playbook -i inventory.ini deploy_web.yml \
                        --private-key $SSH_KEY
                    '''
                }
            }
        }
stage('Deploy to AWS') {
    steps {
        withCredentials([sshUserPrivateKey(credentialsId: 'aws-key-id', keyFileVariable: 'SSH_KEY')]) {
            sh """
                scp -o StrictHostKeyChecking=no -i $SSH_KEY index-aws.html ${AWS_HOST}:/tmp/
                ssh -o StrictHostKeyChecking=no -i $SSH_KEY ${AWS_HOST} '
                    sudo mv /tmp/index-aws.html /var/www/html/index.html &&
                    sudo systemctl restart nginx
                '
            """
        }
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

        stage('Verify Deployment') {
            steps {
                sh '''
                    echo "Checking AWS..."
                    curl -s http://54.197.78.72 | grep "Welcome to AWS"

                    echo "Checking Azure..."
                    curl -s http://13.68.190.21 | grep "Welcome to Azure"
                '''
            }
        }
    }
}
