pipeline {
    agent any

    triggers {
        pollSCM('H/15 * * * *') // Revisa cambios en el repo cada 15 minutos
    }

    environment {
        RUTA_ANALISIS = "/var/jenkins_home/workspace/folderName/subfolderName/projectNameFile"
        IP_LOCAL = "127.0.0.1"
    }

    stages {
        stage('Versiones') {
            steps {
                script {
                    def timestamp = new Date().format("yyyyMMdd_HHmmss")
                    def filename = "versiones_${timestamp}.txt"
                    sh """
                        java -version 2>&1 | tee ${filename}
                        echo 'Jenkins ejecutándose dentro de contenedor Docker. Versión mostrada desde el WAR:' >> ${filename}
                        cat /usr/share/jenkins/jenkins.war > /dev/null 2>&1 && echo 'Jenkins WAR presente' >> ${filename}
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
                    sh "find '${env.RUTA_ANALISIS}' -type f -exec sha256sum {} + > ${filename}"
                }
            }
        }

        stage('Comparación (Manual o Automática)') {
            steps {
                echo "Puedes comparar los archivos generados con 'diff archivo1 archivo2' o con un script Bash."
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
