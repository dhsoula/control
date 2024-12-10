pipeline {
    agent any  // Exécution sur un agent de Jenkins disponible

    environment {
        SONAR_TOKEN = credentials('sonar_token')
        SONAR_HOST_URL = 'http://localhost:9000'  // URL de votre serveur SonarQube
        GITBASH_PATH = 'C:\\Program Files\\Git\\bin\\bash.exe'  // Chemin vers Git Bash avec doubles barres obliques inverses
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Cloner le dépôt Git
                    checkout scm
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Installer les dépendances PHP avec Composer via Git Bash
                    echo "Installation des dépendances PHP avec Composer"
                    bat "\"${env.GITBASH_PATH}\" -c 'composer install'"  // Utilisation de Git Bash pour exécuter Composer
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Exécuter l'analyse SonarQube avec sonar-scanner via Git Bash
                    echo "Exécution de l'analyse SonarQube"
                    bat "\"${env.GITBASH_PATH}\" -c 'sonar-scanner -Dsonar.projectKey=control -Dsonar.sources=. -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.login=$SONAR_TOKEN'"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Déployer l'application (commande Windows)
                    echo "Déploiement de l'application"
                    bat "\"${env.GITBASH_PATH}\" -c 'mkdir -p C:/path/to/production/folder && xcopy /e /i /h * C:/path/to/production/folder'"
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
