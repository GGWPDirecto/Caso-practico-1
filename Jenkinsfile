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
        stage('Tests Paralelos') {
            parallel {
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
                stage('Rest') {
                    steps {
                        bat '''
                            set FLASK_APP=app\\api.py
                            set FLASK_ENV=development
                            start flask run
                            start java -jar C:\\WireMock\\wiremock-standalone-3.13.0.jar --port 9090 --root-dir C:\\WireMock
                            set PYTHONPATH=%WORKSPACE%
                            pytest --junitxml=result-rest.xml test\\rest
                        '''
                    }
                }
            }
        }
        
        stage('results'){
            steps{
                junit 'result*.xml'
            }
        }
    }
}
