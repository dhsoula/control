pipeline {
    agent any  // Exécution sur un agent de Jenkins disponible

    environment {
        SONAR_TOKEN = credentials('sonar_token')  // Récupération du token SonarQube depuis les credentials
        SONAR_HOST_URL = 'http://localhost:9000'  // URL de votre serveur SonarQube
        GITBASH_PATH = 'C:\\Program Files\\Git\\bin\\bash.exe'  // Chemin vers Git Bash avec doubles barres obliques inverses
    }

    stages {
        // Étape 1: Checkout du code source
        stage('Checkout') {
            steps {
                script {
                    // Cloner le dépôt Git
                    checkout scm
                }
            }
        }

        // Étape 2: Installation des dépendances avec Composer via Git Bash
        stage('Install Dependencies') {
            steps {
                script {
                    echo "Installation des dépendances PHP avec Composer"
                    // Exécution de Composer via Git Bash en utilisant sh
                    sh "\"${env.GITBASH_PATH}\" -c 'composer install'"
                }
            }
        }

        // Étape 3: Analyse SonarQube via Git Bash
        stage('SonarQube Analysis') {
            steps {
                script {
                    echo "Exécution de l'analyse SonarQube"
                    // Exécution du scanner SonarQube via Git Bash
                    sh "\"${env.GITBASH_PATH}\" -c 'sonar-scanner -Dsonar.projectKey=control -Dsonar.sources=. -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.login=$SONAR_TOKEN'"
                }
            }
        }

        // Étape 4: Déploiement de l'application
        stage('Deploy') {
            steps {
                script {
                    echo "Déploiement de l'application"
                    // Création du dossier de production et copie des fichiers
                    sh "\"${env.GITBASH_PATH}\" -c 'mkdir -p C:/path/to/production/folder && xcopy /e /i /h * C:/path/to/production/folder'"
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
