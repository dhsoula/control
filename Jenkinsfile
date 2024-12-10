pipeline {
    agent any  // Exécution sur l'agent de Jenkins sans utiliser Docker

    environment {
        SONAR_TOKEN = credentials('sonar_token')
        SONAR_HOST_URL = 'http://localhost:9000'  // URL de votre serveur SonarQube
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Cloner le dépôt Git dans Jenkins
                    checkout scm
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Installer Composer et les dépendances PHP
                    echo "Installation des dépendances PHP avec Composer"
                    sh 'composer install'  // Assurez-vous que Composer est installé sur l'agent Jenkins
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Exécuter l'analyse SonarQube avec sonar-scanner
                    echo "Exécution de l'analyse SonarQube"
                    sh '''
                        sonar-scanner \
                        -Dsonar.projectKey=control \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=$SONAR_HOST_URL \
                        -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Déployer l'application dans un répertoire de production
                    echo "Déploiement de l'application"
                    sh 'mkdir -p /path/to/production/folder && cp -r * /path/to/production/folder'
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
