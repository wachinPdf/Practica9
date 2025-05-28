pipeline {
    agent any
    stages {
        stage('Install') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    sh 'npm install'
                }
            }
        }
        stage('Test') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    sh 'npm test'
                }
            }
        }
        stage('Deploy to Docker') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    sh '''
                    docker build -t my-react-app .
                    docker run -d -p 3000:3000 --rm my-react-app || true
                    '''
                }
            }
        }
    }
    post {
        always {
            echo 'CD pipeline finalizado'
        }
    }
}
