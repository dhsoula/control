pipeline {
    agent any
    
    environment {
        SONAR_TOKEN = credentials('sonar_token')  // Utilisation du token SonarQube
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
                    // Utilisez des commandes compatibles Windows
                    bat 'composer install'  // Cette commande fonctionne sous Windows
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Commande SonarScanner pour analyser le code PHP
                    bat "sonar-scanner -Dsonar.projectKey=control -Dsonar.host.url=http://localhost:9000 -Dsonar.login=${SONAR_TOKEN}"
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
