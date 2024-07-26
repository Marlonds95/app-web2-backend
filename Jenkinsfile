node {
    // Define las variables de entorno
    def pythonPath = 'C:\\Users\\EQUIPO\\AppData\\Local\\Programs\\Python\\Python312\\python.exe'
    def emailScriptPath = 'C:\\ProgramData\\Jenkins\\.jenkins\\scripts\\send_email.py'

    try {
        stage('Revisión') {
            // Checkout del código fuente desde el repositorio bifurcado
            checkout([$class: 'GitSCM', 
                      branches: [[name: 'master']],
                      userRemoteConfigs: [[url: 'https://github.com/Marlonds95/app-web2-backend.git']]])
        }

        stage('Instalación de Maven') {
            // Verifica la versión de Maven
            bat 'mvn --version'
        }

        stage('Instalación de dependencias') {
            // Instala las dependencias del proyecto usando Maven
            bat 'mvn install'
        }

        stage('Construir') {
            // Construye el proyecto usando Maven
            bat 'mvn package'
        }

        stage('Limpiar') {
            // Limpia la carpeta en el servidor
            bat 'rd /s /q C:\\servidor\\fire'
        }

        stage('Mover al servidor') {
            // Copia los archivos construidos al servidor
            bat 'xcopy C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\app-web2-backend\\target\\app-web2-backend.jar C:\\servidor\\fire /Y'
        }

    } catch (Exception e) {
        // Manejo de errores en caso de que algo falle en las etapas
        currentBuild.result = 'FAILURE'
        throw e
    } finally {
        if (currentBuild.result == 'SUCCESS') {
            stage('Enviar correo') {
                // Ejecuta el script de Python para enviar un correo electrónico solo si el build es exitoso
                bat "${pythonPath} ${emailScriptPath}"
            }
        }
    }
}
