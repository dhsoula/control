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
                    // Commande pour installer les dépendances PHP sous Windows
                    bat 'composer install'  // Assurez-vous que Composer est installé sur votre machine
                }
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Utilisation de Docker pour exécuter SonarScanner avec votre projet
                    bat """
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
                    // Déploiement dans le dossier de production (sous Windows)
                    bat 'xcopy * /path/to/production/folder /E /H /Y'
                }
            }
        }
    }
}
