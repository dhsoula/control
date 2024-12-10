pipeline {
    agent any  // Utilise n'importe quel nœud disponible

    environment {
        SONAR_TOKEN = credentials('sonar_token')
        SONAR_HOST_URL = 'http://localhost:9000'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/dhsoula/control.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    // Utiliser 'bat' pour exécuter des commandes Windows
                    bat 'composer install'  // Remplace sh par bat
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    bat """
                    sonar-scanner ^
                    -Dsonar.projectKey=control ^
                    -Dsonar.sources=. ^
                    -Dsonar.host.url=%SONAR_HOST_URL% ^
                    -Dsonar.login=%SONAR_TOKEN%
                    """
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    bat """
                    mkdir C:\\path\\to\\production\\folder
                    xcopy /E /I * C:\\path\\to\\production\\folder
                    """
                }
            }
        }
    }
}

