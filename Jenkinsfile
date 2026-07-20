<<<<<<< HEAD
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
                echo 'Deploying to Development Server'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to Staging Server'
            }
        }

        stage('Manual Approval') {
            steps {
                input message: 'Approve Production Deployment?', ok: 'Deploy'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to Production Server'
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
=======
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
                echo 'Deploying to Development Server'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to Staging Server'
            }
        }

        stage('Manual Approval') {
            steps {
                input message: 'Approve Production Deployment?', ok: 'Deploy'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to Production Server'
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
>>>>>>> c9375fa (Added sample web application)
