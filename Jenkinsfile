pipeline {
    agent any

    stages {
        stage('Update Dependencies') {
            steps {
                script {
                    echo 'Updating dependencies...'
                    sh 'npm install npm-check-updates'
                    sh 'npx npm-check-updates -u'
                    sh 'npm install --legacy-peer-deps'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Building the project...'
                    sh 'npm install --only=dev'
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
