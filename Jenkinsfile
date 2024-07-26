node {
    // Define las variables de entorno
    
    def mavenPath = 'C:\\Maven\\apache-maven-3.9.8\\bin\\mvn.cmd'

    try {
        stage('Revisión') {
            // Checkout del código fuente desde el repositorio bifurcado
            checkout([$class: 'GitSCM', 
                      branches: [[name: 'master']],
                      userRemoteConfigs: [[url: 'https://github.com/Marlonds95/app-web2-backend.git']]])
        }

        stage('Instalación de Maven') {
            // Verifica la versión de Maven
            bat "${mavenPath} --version"
        }

        stage('Instalación de dependencias') {
            // Instala las dependencias del proyecto usando Maven
            bat "${mavenPath} install"
        }

        stage('Construir') {
            // Construye el proyecto usando Maven
            bat "${mavenPath} package"
        }

        stage('Limpiar') {
            // Limpia la carpeta en el servidor
            bat 'rd /s /q C:\\servidor\\fire'
        }

        stage('Mover al servidor') {
            // Copia los archivos construidos al servidor
            bat 'xcopy C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\Actividad-pipe-Angular\\dist\\app-03\\browser C:\\servidor\\fire /E /I /Y'
        }

    } catch (Exception e) {
        // Manejo de errores en caso de que algo falle en las etapas
        currentBuild.result = 'FAILURE'
        throw e
    } finally {
        if (currentBuild.result == 'SUCCESS') {
            
            }
        }
    }
}
