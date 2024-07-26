node {
    // Define la variable de entorno para Maven
    def mavenPath = 'C:\\Maven\\apache-maven-3.9.8\\bin\\mvn.cmd'
    def targetDir = 'C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\app-web2-backend\\target'
    def gitRepo = 'https://github.com/Marlonds95/app-web2-backend.git'
    def workspaceDir = 'C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\app-web2-backend'

    try {
        stage('Clonación del Repositorio') {
            // Clona el repositorio en el directorio de trabajo
            bat "git clone ${gitRepo} ${workspaceDir}"
        }

        stage('Revisión') {
            // Mueve al directorio del proyecto clonado
            dir(workspaceDir) {
                // Checkout del código fuente desde el repositorio bifurcado
                checkout([$class: 'GitSCM', 
                          branches: [[name: 'master']],
                          userRemoteConfigs: [[url: gitRepo]]])
            }
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
            // Limpia la carpeta target si existe
            bat "if exist ${targetDir} rd /s /q ${targetDir}"
        }

    } catch (Exception e) {
        // Manejo de errores en caso de que algo falle en las etapas
        currentBuild.result = 'FAILURE'
        throw e
    }
}


