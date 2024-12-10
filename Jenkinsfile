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
                    // Installation des dépendances PHP avec Composer, incluant PHPUnit dans "require-dev"
                    sh 'composer install --dev'  // Installe les dépendances définies dans composer.json (y compris PHPUnit)
                }
            }
        }

        // Etape pour exécuter les tests PHPUnit
        stage('Run Tests') {
            steps {
                script {
                    // Exécution des tests PHPUnit après l'installation des dépendances
                    sh 'vendor/bin/phpunit --config phpunit.xml'  // Exécution des tests PHPUnit avec le fichier de configuration
                }
            }
        }

        // Etape pour l'analyse de code avec SonarQube
        stage('Code Analysis') {
            steps {
                withSonarQubeEnv('SonarQube-Server') { // Assurez-vous que SonarQube est bien configuré dans Jenkins sous ce nom
                    script {
                        // Commande pour exécuter l'analyse de SonarQube avec le token récupéré
                        sh '''
                            sonar-scanner
                                -Dsonar.projectKey=tp-jenkins
                                -Dsonar.sources=.
                                -Dsonar.tests=test
                                -Dsonar.host.url=http://localhost:9000
                                -Dsonar.token=${SONAR_TOKEN}  // Utilisation du token SonarQube récupéré des credentials Jenkins
                        '''
                    }
                }
            }
        }
    }
}
