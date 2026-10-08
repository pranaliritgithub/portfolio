pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/pranaliritgithub/portfolio.git'
            }
        }

        stage('Deploy Portfolio') {
            steps {

                sshagent(['node-pipeline-key']) {

                    sh '''
                        ssh -o StrictHostKeyChecking=no \
                        ubuntu@172.31.43.196 \
                        "sudo mkdir -p /var/www/html"

                        scp -o StrictHostKeyChecking=no \
                        index.html \
                        ubuntu@172.31.43.196:/tmp/index.html

                        ssh -o StrictHostKeyChecking=no \
                        ubuntu@172.31.43.196 \
                        "sudo cp /tmp/index.html /var/www/html/index.html && \
                         sudo chmod 644 /var/www/html/index.html"
                    '''
                }
            }
        }

        stage('Verify Deployment') {
            steps {

                sshagent(['node-pipeline-key']) {

                    sh '''
                        ssh -o StrictHostKeyChecking=no \
                        ubuntu@172.31.43.196 \
                        "curl -I http://localhost"
                    '''
                }
            }
        }
    }

    post {

        success {
            echo 'Portfolio deployed successfully!'
        }

        failure {
            echo 'Portfolio deployment failed!'
        }
    }
}
