pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar_token')  // Token pour SonarQube
        SONAR_HOST_URL = 'http://localhost:9000'
        GIT_BASH = "C:/Program Files/Git/bin/bash.exe"  // Chemin vers Git Bash
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: '5f144011-1480-4623-ae64-dc4d512d9045', branch: 'master', url: 'https://github.com/dhsoula/control.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    // Utilisation de Git Bash pour exécuter la commande Composer
                    sh "\"${env.GIT_BASH}\" -c 'composer install'"
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Utilisation de Git Bash pour exécuter sonar-scanner
                    sh """
                    \"${env.GIT_BASH}\" -c 'sonar-scanner ^
                    -Dsonar.projectKey=control ^
                    -Dsonar.sources=. ^
                    -Dsonar.host.url=${env.SONAR_HOST_URL} ^
                    -Dsonar.login=${env.SONAR_TOKEN}'
                    """
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Création du dossier et copie des fichiers de déploiement
                    sh """
                    \"${env.GIT_BASH}\" -c 'mkdir -p /c/path/to/production/folder'
                    \"${env.GIT_BASH}\" -c 'cp -r * /c/path/to/production/folder'
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
