pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Clone Repo') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/shitesh372/project.git'
            }
        }

        stage('Deploy to AWS') {
            steps {
                sh '''
                  export ANSIBLE_HOST_KEY_CHECKING=False
                  ansible aws -i inventory.ini -m copy \
                    -a "src=index-aws.html dest=/var/www/html/index.html owner=www-data group=www-data mode=0644" \
                    --become --become-user=root
                '''
            }
        }

        stage('Deploy to Azure') {
            steps {
                sh '''
                  export ANSIBLE_HOST_KEY_CHECKING=False
                  ansible azure -i inventory.ini -m copy \
                    -a "src=index-azure.html dest=/var/www/html/index.html owner=www-data group=www-data mode=0644" \
                    --become --become-user=root
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                  echo "Verifying AWS deployment..."
                  curl -s http://100.56.101.120/index.html | grep "AWS"

                  echo "Verifying Azure deployment..."
                  curl -s http://52.172.45.33/index.html | grep "Azure"
                '''
            }
        }
    }
}
