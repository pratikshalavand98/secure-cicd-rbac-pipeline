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


        stage('Environment Check') {
            steps {
                sh "whoami"
                sh "hostname"
                sh "date"
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

                echo "Deploying to Development Environment"

                sshagent(credentials:['ec2-ssh-key']) {

                    sh """

                    scp -o StrictHostKeyChecking=no \
                    app/index.html \
                    ubuntu@${DEV_SERVER}:/tmp/index.html


                    ssh -o StrictHostKeyChecking=no ubuntu@${DEV_SERVER} '

                    sudo cp /tmp/index.html /var/www/html/index.html

                    sudo systemctl restart nginx

                    '

                    """
                }
            }
        }



        stage('Deploy to Staging') {

            steps {

                echo "Deploying to Staging Environment"

                sshagent(credentials:['ec2-ssh-key']) {

                    sh """

                    scp -o StrictHostKeyChecking=no \
                    app/index.html \
                    ubuntu@${STAGING_SERVER}:/tmp/index.html


                    ssh -o StrictHostKeyChecking=no ubuntu@${STAGING_SERVER} '

                    sudo cp /tmp/index.html /var/www/html/index.html

                    sudo systemctl restart nginx

                    '

                    """
                }
            }
        }




        stage('Manual Approval') {

            steps {

                timeout(time:30, unit:'MINUTES') {

                    input(
                    message:'Approve Production Deployment?',
                    ok:'Deploy'
                    )

                }
            }
        }





        stage('Deploy to Production') {

            steps {

                echo "Deploying Production Environment"


                sshagent(credentials:['ec2-ssh-key']) {

                    sh """

                    scp -o StrictHostKeyChecking=no \
                    app/index.html \
                    ubuntu@${PROD_SERVER}:/tmp/index.html


                    ssh -o StrictHostKeyChecking=no ubuntu@${PROD_SERVER} '

                    sudo cp /tmp/index.html /var/www/html/index.html

                    sudo systemctl restart nginx

                    '

                    """
                }
            }
        }

    }


    post {

        success {

            echo "CI/CD Pipeline Completed Successfully"

        }


        failure {

            echo "CI/CD Pipeline Failed"

        }

    }
}
