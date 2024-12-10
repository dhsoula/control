pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'SonarQube-Server'  // Assurez-vous que cela correspond au nom de votre serveur SonarQube dans Jenkins
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone le dépôt GitHub
                git 'https://github.com/username/repository.git'  // Remplacez par l'URL de votre dépôt GitHub
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
                    // Exécute vos tests (par exemple avec PHPUnit)
                    sh 'vendor/bin/phpunit'  // Assurez-vous que PHPUnit est installé via Composer
                }
            }
        }

        stage('Code Analysis') {
            steps {
                withSonarQubeEnv('SonarQube-Server') {
                    // Exécute l'analyse de code avec SonarQube
                    sh '''
                    ./vendor/bin/sonar-scanner ^
                      -Dsonar.projectKey=tp-jenkins ^
                      -Dsonar.sources=. ^  // Dossier contenant le code source, incluant vos fichiers PHP
                      -Dsonar.tests=tests ^  // Dossier contenant vos tests
                      -Dsonar.host.url=http://localhost:9000 ^
                      -Dsonar.token=sqp_741334ed5307b9bbba5b6715ca354d9f705f7baf
                    '''
                }
            }
        }
    }
}
