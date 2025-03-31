pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building the project...'
                    sh 'npm cache clean --force'
                    sh 'npm install --legacy-peer-deps'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    sh 'npm test'
                }
            }
        }
    }

    post {
        success {
            echo 'CI process completed successfully.'
        }
        failure {
            echo 'CI process failed.'
        }
    }
}
