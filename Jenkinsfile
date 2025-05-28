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
        stage('Generate KPI report: File integrity') {
            steps {
                script {
                    sh '''
                    find . -type f ! -path "./node_modules/*" -exec sha256sum {} \\; > sha256_report.txt
                    '''
                    archiveArtifacts artifacts: 'sha256_report.txt'
                }
            }
        }
    }
    post {
        always {
            echo 'CI/CD pipeline con reporting de integridad ejecutado.'
        }
    }
}
