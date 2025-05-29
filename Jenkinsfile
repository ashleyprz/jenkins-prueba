pipeline {
    agent any

    stages {
        stage('Clonar repositorio') {
            steps {
                git url: 'https://github.com/ashleyprz/jenkins-prueba.git', branch: 'main', credentialsId: 'github-token'
            }
        }

        stage('Instalar dependencias') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Ejecutar pruebas') {
            steps {
                sh 'pytest'
            }
        }
    }
}