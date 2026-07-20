pipeline {
    agent any

    environment {
        DEV_SERVER = "44.201.216.9"
        STAGING_SERVER = "44.211.168.120"
        PROD_SERVER = "13.222.41.40"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Building Application..."
                sh "ls -la"
                sh "ls -la app"
            }
        }

        stage('Unit Test') {
            steps {
                echo "Running Unit Tests..."
            }
        }

        stage('Deploy to Dev') {
            steps {
                echo "Deploying to Development Server..."

                sshagent(credentials: ['ec2-ssh-key']) {
                    sh """
                    scp -o StrictHostKeyChecking=no app/index.html ubuntu@${DEV_SERVER}:/tmp/index.html

                    ssh -o StrictHostKeyChecking=no ubuntu@${DEV_SERVER} '
                        sudo cp /tmp/index.html /var/www/html/index.html
                        sudo chown www-data:www-data /var/www/html/index.html
                        sudo systemctl restart nginx
                    '
                    """
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo "Deploying to Staging Server..."

                sshagent(credentials: ['ec2-ssh-key']) {
                    sh """
                    scp -o StrictHostKeyChecking=no app/index.html ubuntu@${STAGING_SERVER}:/tmp/index.html

                    ssh -o StrictHostKeyChecking=no ubuntu@${STAGING_SERVER} '
                        sudo cp /tmp/index.html /var/www/html/index.html
                        sudo chown www-data:www-data /var/www/html/index.html
                        sudo systemctl restart nginx
                    '
                    """
                }
            }
        }

        stage('Manual Approval') {
            steps {
                input(
                    message: 'Approve Production Deployment?',
                    ok: 'Deploy'
                )
            }
        }

        stage('Deploy to Production') {
            steps {
                echo "Deploying to Production Server..."

                sshagent(credentials: ['ec2-ssh-key']) {
                    sh """
                    scp -o StrictHostKeyChecking=no app/index.html ubuntu@${PROD_SERVER}:/tmp/index.html

                    ssh -o StrictHostKeyChecking=no ubuntu@${PROD_SERVER} '
                        sudo cp /tmp/index.html /var/www/html/index.html
                        sudo chown www-data:www-data /var/www/html/index.html
                        sudo systemctl restart nginx
                    '
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline Completed Successfully."
        }

        failure {
            echo "Pipeline Failed."
        }
    }
}
