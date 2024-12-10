pipeline {
    agent any  // Utilise n'importe quel nœud disponible

    environment {
        SONAR_TOKEN = credentials('sonar_token')  // Assurez-vous que 'sonar_token' est bien configuré dans Jenkins
        SONAR_HOST_URL = 'http://localhost:9000' // URL de votre instance SonarQube
    }

    stages {
        stage('Checkout') {
            steps {
                // Télécharge le code depuis le dépôt Git
                git branch: 'main', url: 'https://github.com/dhsoula/control.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    // Exécute des commandes Windows pour installer les dépendances avec Composer
                    bat 'composer install'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Analyse le code avec SonarQube
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
                    // Copie les fichiers dans le dossier de production
                    bat """
                    mkdir C:\\path\\to\\production\\folder
                    xcopy /E /I * C:\\path\\to\\production\\folder
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline exécutée avec succès !'
        }
        failure {
            echo 'Pipeline échouée. Vérifiez les logs pour plus de détails.'
        }
    }
}
