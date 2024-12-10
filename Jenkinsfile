pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar_token')  // Utilisation du token SonarQube
        SONAR_HOST_URL = 'http://localhost:9000'  // URL de votre serveur SonarQube
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
                    // Utilisation d'un conteneur PHP pour installer les dépendances
                    sh """
                        docker run --rm \
                        -v ${WORKSPACE}:/app \
                        -w /app \
                        php:8.2-cli composer install
                    """
                }
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Utilisation de Docker pour exécuter SonarScanner
                    sh """
                        docker run --rm \
                        -e SONAR_TOKEN=${SONAR_TOKEN} \
                        -e SONAR_HOST_URL=${SONAR_HOST_URL} \
                        -v ${WORKSPACE}:/usr/src sonarsource/sonar-scanner-cli
                    """
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    // Déploiement (Linux)
                    sh """
                        cp -r ${WORKSPACE}/* /path/to/production/folder
                    """
                }
            }
        }
    }
}
