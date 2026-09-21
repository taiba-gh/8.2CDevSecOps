pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/taiba-gh/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
            post {
                success {
                    emailext(
                        to: params.NOTIFICATION_EMAIL,
                        subject: "SUCCESS: Run Tests - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The Run Tests stage completed successfully. The Jenkins console log is attached.",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: params.NOTIFICATION_EMAIL,
                        subject: "FAILURE: Run Tests - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The Run Tests stage failed. The Jenkins console log is attached.",
                        attachLog: true
                    )
                }
            }
        }

        stage('Generate Coverage Report') {
            steps {
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true'
            }
            post {
                success {
                    emailext(
                        to: params.NOTIFICATION_EMAIL,
                        subject: "SUCCESS: Security Scan - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The NPM Audit security scan completed successfully. The Jenkins console log is attached.",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: params.NOTIFICATION_EMAIL,
                        subject: "FAILURE: Security Scan - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The NPM Audit security scan failed. The Jenkins console log is attached.",
                        attachLog: true
                    )
                }
            }
        }
    }
}
