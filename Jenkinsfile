pipeline {
    agent any
    environment {
        // Récupération du token SonarQube à partir des credentials Jenkins
        SONAR_TOKEN = credentials('sonar_token')  // Utilisation du secret 'sonar_token' stocké dans Jenkins
    }
    stages {
        // Etape pour cloner le dépôt depuis GitHub
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        // Etape pour installer les dépendances avec Composer
        stage('Install Dependencies') {
            steps {
                script {
                    // Installation des dépendances PHP avec Composer
                    sh 'composer install --no-interaction --prefer-dist'  // Installe les dépendances définies dans composer.json
                }
            }
        }

        // Etape pour exécuter les tests PHPUnit
        stage('Run Tests') {
            steps {
                script {
                    // Modifier les permissions du fichier phpunit pour le rendre exécutable
                    sh 'chmod +x vendor/bin/phpunit'
                    // Exécuter les tests PHPUnit en utilisant le fichier de configuration phpunit.xml
                    sh 'vendor/bin/phpunit --config phpunit.xml'
                }
            }
        }

        // Etape pour l'analyse de code avec SonarQube
        stage('Code Analysis') {
            steps {
                withSonarQubeEnv('SonarQube server') {  // Assurez-vous que SonarQube est bien configuré dans Jenkins sous ce nom
                    script {
                        // Exécuter l'analyse SonarQube
                        sonarScanner(
                            projectKey: 'tp-jenkinse',
                            projectName: 'My PHP Project',
                            sources: '.',
                            tests: 'tests',
                            login: "${SONAR_TOKEN}"
                        )
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

