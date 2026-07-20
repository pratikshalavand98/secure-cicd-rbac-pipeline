pipeline {
    agent any

    environment {
        DEV_SERVER = "44.201.216.9"
        STAGING_SERVER = "44.211.168.120"
        PROD_SERVER = "13.222.41.40"
    }

    stages {

        stage('Build') {
            steps {
                echo 'Building Application...'
            }
        }

        stage('Unit Test') {
            steps {
                echo 'Running Unit Tests...'
            }
        }

        stage('Deploy to Dev') {
            steps {
                sshagent(credentials: ['ec2-ssh-key']) {
                    sh '''
                    scp -o StrictHostKeyChecking=no app/index.html ubuntu@$DEV_SERVER:/tmp/index.html
                    ssh -o StrictHostKeyChecking=no ubuntu@$DEV_SERVER "sudo mv /tmp/index.html /var/www/html/index.html"
                    '''
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                sshagent(credentials: ['ec2-ssh-key']) {
                    sh '''
                    scp -o StrictHostKeyChecking=no app/index.html ubuntu@$STAGING_SERVER:/tmp/index.html
                    ssh -o StrictHostKeyChecking=no ubuntu@$STAGING_SERVER "sudo mv /tmp/index.html /var/www/html/index.html"
                    '''
                }
            }
        }

        stage('Manual Approval') {
            steps {
                input message: 'Approve Production Deployment?', ok: 'Deploy'
            }
        }

        stage('Deploy to Production') {
            steps {
                sshagent(credentials: ['ec2-ssh-key']) {
                    sh '''
                    scp -o StrictHostKeyChecking=no app/index.html ubuntu@$PROD_SERVER:/tmp/index.html
                    ssh -o StrictHostKeyChecking=no ubuntu@$PROD_SERVER "sudo mv /tmp/index.html /var/www/html/index.html"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline Completed Successfully'
        }

        failure {
            echo 'Pipeline Failed'
        }
    }
}
