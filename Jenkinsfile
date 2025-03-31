pipeline {
    agent {
        docker {
            image 'node:16-alpine'  // Contenedor ligero con Node.js
            args '-u root'  // Evita problemas de permisos
        }
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm  // Clona el repositorio
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test'  // Ejecuta pruebas (Jest/Enzyme)
            }
            post {
                always {
                    junit 'reports/**/*.xml'  // Guarda reportes de pruebas
                }
            }
        }
    }
    triggers {
        pollSCM('H/5 * * * *')  // Verifica cambios cada 5 min (alternativa a webhooks)
    }
}