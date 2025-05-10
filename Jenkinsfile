pipeline {
    agent any
    
    stages {
        stage('Get Code') {
            steps {
                echo 'Descargando el código del repositorio'
                git 'https://github.com/GGWPDirecto/Caso-practico-1.git'
                bat 'dir'
                echo env.WORKSPACE
            }
        }
        stage('Build') {
            steps {
                echo "NO ES NECESARIO COMPILAR YA QUE ES PYTHON"
            }
        }
        stage('Unit') {
            steps {
                bat 'where python'
                bat 'python --version'

                bat '''
                    set PYTHONPATH=%WORKSPACE%
                    pytest --junitxml=result-unit.xml test\\unit
                '''
            }
        }
    }
}
