pipeline {
    agent any  // Usa cualquier agente disponible
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build and Test') {
            steps {
                script {
                    // Ejecuta en un contenedor Docker ad-hoc
                    docker.image('node:16-alpine').inside('-u root') {
                        sh 'npm install'
                        sh 'npm test'
                    }
                }
            }
            post {
                always {
                    junit 'reports/**/*.xml'  // Reporte de pruebas
                }
            }
        }
    }
    triggers {
        pollSCM('H/5 * * * *')  // Verifica cambios cada 5 min
    }
}