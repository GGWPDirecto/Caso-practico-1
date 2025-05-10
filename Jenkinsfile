pipeline {
    agent none

    stages {
        stage('Get Code') {
            agent {
                label 'Agente1'
            }
            steps {
                echo 'Descargando el código del repositorio'
                git 'https://github.com/GGWPDirecto/Caso-practico-1.git'
                bat 'dir'
                echo env.WORKSPACE
            }
        }
        stage('Build') {
            agent {
                label 'Agente1'
            }
            steps {
                echo "NO ES NECESARIO COMPILAR YA QUE ES PYTHON"
            }
        }
        stage('Tests Paralelos') {
            parallel {
                stage('Unit') {
                    agent {
                        label 'Agente1'
                    }
                    steps {
                        bat 'where python'
                        bat 'python --version'
                        bat '''
                            set PYTHONPATH=%WORKSPACE%
                            pytest --junitxml=result-unit.xml test\\unit
                        '''
                        stash name: 'unit-results', includes: 'result-unit.xml'
                    }
                }
                stage('Rest') {
                    agent {
                        label 'agente 2'
                    }
                    steps {
                        bat '''
                            set FLASK_APP=app\\api.py
                            set FLASK_ENV=development
                            start flask run
                            start java -jar C:\\WireMock\\wiremock-standalone-3.13.0.jar --port 9090 --root-dir C:\\WireMock
                            set PYTHONPATH=%WORKSPACE%
                            pytest --junitxml=result-rest.xml test\\rest
                        '''
                        stash name: 'rest-results', includes: 'result-rest.xml'
                    }
                }
            }
        }
        stage('Results') {
            agent {
                label 'Agente1'
            }
            steps {
                unstash 'unit-results'
                unstash 'rest-results'
                junit 'result*.xml'
            }
        }
    }
}
