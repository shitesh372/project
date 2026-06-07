pipeline {
    agent any

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
                        ansible-playbook -i inventory.ini deploy_web.yml \
                        --private-key $SSH_KEY
                    '''
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    echo "Checking AWS..."
                    curl -s http://100.51.3.180 | grep "Welcome to AWS"

                    echo "Checking Azure..."
                    curl -s http://74.235.8.57 | grep "Welcome to Azure"
                '''
            }
        }
    }
}
