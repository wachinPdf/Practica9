pipeline {
    agent any

    triggers {
        pollSCM('H/15 * * * *')
    }

    environment {
        RUTA_ANALISIS = "C:/ProgramData/Jenkins/.jenkins/workspace/folderName/subfolderName/projectNameFile"
        IP_LOCAL = "127.0.0.1"
    }

    stages {
        stage('Versiones') {
            steps {
                script {
                    def timestamp = new Date().format("yyyyMMdd_HHmmss")
                    def filename = "versiones_${timestamp}.txt"
                    powershell """
                        java -version 2>&1 | Out-File -FilePath ${filename} -Append
                        echo '---' | Out-File -FilePath ${filename} -Append
                        jenkins --version | Out-File -FilePath ${filename} -Append
                    """
                }
            }
        }

        stage('Escaneo de Puertos') {
            steps {
                script {
                    def timestamp = new Date().format("yyyyMMdd_HHmmss")
                    def filename = "scan_${timestamp}.txt"
                    powershell "nmap ${env.IP_LOCAL} -p- | Out-File ${filename}"
                }
            }
        }

        stage('SHA-256 de Ficheros') {
            steps {
                script {
                    def timestamp = new Date().format("yyyyMMdd_HHmmss")
                    def filename = "sha256_${timestamp}.txt"
                    powershell "Get-ChildItem -Recurse '${env.RUTA_ANALISIS}' | Get-FileHash -Algorithm SHA256 | Out-File ${filename}"
                }
            }
        }

        stage('Comparación (Manual o Automática)') {
            steps {
                echo "Puedes comparar los archivos generados manualmente con Notepad++, fc o un script PowerShell."
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
