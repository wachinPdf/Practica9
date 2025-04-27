pipeline {
    agent any

    tools {
        nodejs 'NodeJS' // Requiere que tengas NodeJS configurado en Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests with Coverage') {
            steps {
                sh 'npm run test -- --coverage --watchAll=false'
            }
        }
        stage('Publish Coverage Report') {
            steps {
                publishHTML([
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'coverage/lcov-report',
                    reportFiles: 'index.html',
                    reportName: 'Coverage Report'
                ])
            }
        }
    }
}
