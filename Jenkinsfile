pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token')  // Utilise le token SonarQube stocké dans Jenkins sous 'sonar-token'
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone le dépôt GitHub
                git 'https://github.com/dhsoula/control.git'  // Remplacez par l'URL de votre dépôt
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Installe les dépendances PHP via Composer
                    sh 'composer install'  // Utilisez 'sh' pour les environnements Unix, remplacez par 'bat' pour Windows si nécessaire
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Exécute les tests unitaires avec PHPUnit
                    sh 'vendor/bin/phpunit'  // Assurez-vous que PHPUnit est installé via Composer
                }
            }
        }

        stage('Code Analysis') {
            steps {
                // Exécute l'analyse de code avec SonarQube
                sh '''
                ./vendor/bin/sonar-scanner ^
                  -Dsonar.projectKey=control-project ^
                  -Dsonar.sources=. ^  // Dossier contenant le code source, incluant vos fichiers PHP
                  -Dsonar.tests=tests ^  // Dossier contenant vos tests
                  -Dsonar.host.url=http://localhost:9000 ^
                  -Dsonar.token=$SONAR_TOKEN  // Utilise le token stocké dans les credentials Jenkins
                '''
            }
        }
    }
}
