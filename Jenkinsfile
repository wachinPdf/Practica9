pipeline {
    agent any

    triggers {
        pollSCM('H/15 * * * *')
    }

    environment {
        RUTA_ANALISIS = "JENKINS_HOME/workspace/folderName/subfolderName/projectNameFile"
        IP_LOCAL = "127.0.0.1" // puedes cambiarla si tu contenedor usa otra IP
    }

    stages {
        stage('Versiones') {
            steps {
                script {
                    def timestamp = new Date().format("yyyyMMdd_HHmmss")
                    def filename = "versiones_${timestamp}.txt"
                    writeFile file: filename, text: """
JAVA VERSION:
${sh(script: 'java -version 2>&1', returnStdout: true)}

JENKINS VERSION:
${sh(script: 'jenkins-cli version', returnStdout: true)}
"""
                }
            }
        }

        stage('Escaneo de Puertos') {
            steps {
                script {
                    def timestamp = new Date().format("yyyyMMdd_HHmmss")
                    def filename = "scan_${timestamp}.txt"
                    sh "nmap ${env.IP_LOCAL} -p- > ${filename}"
                }
            }
        }

        stage('SHA-256 de Ficheros') {
            steps {
                script {
                    def timestamp = new Date().format("yyyyMMdd_HHmmss")
                    def filename = "sha256_${timestamp}.txt"
                    sh "find ${env.RUTA_ANALISIS} -type f -exec sha256sum {} \\; > ${filename}"
                }
            }
        }

        stage('Comparación (Manual o Automática)') {
            steps {
                script {
                    echo "Puedes comparar dos snapshots usando diff, fc o un script externo."
                    echo "Ejemplo: diff sha256_20250407_141000.txt sha256_20250407_143000.txt"
                }
            }
        }
    }

    post {
        success {
            echo 'Análisis de seguridad finalizado con éxito.'
        }
        failure {
            echo 'El análisis de seguridad ha fallado.'
        }
    }
}
