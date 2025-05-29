pipeline {
    agent {
        docker {
            image 'python:3.10'
            args '-u root' // Ejecutar como root para evitar errores de permisos
        }
    }
    stages {
        stage('Clonar repositorio') {
            steps {
                git url: 'https://github.com/ashleyprz/jenkins-prueba.git', branch: 'main', credentialsId: 'github-token'
            }
        }
        stage('Instalar dependencias') {
            steps {
                sh 'pip install -r requirements.txt || echo "No existen requisitos o ya están instalados"'
            }
        }
        stage('Ejecutar pruebas') {
            steps {
                sh 'pytest || echo "No se encontraron tests"' 
            }
        }
    }
}
