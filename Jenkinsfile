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
                        // Exécuter l'analyse de SonarQube avec le token récupéré
                        sh '''
                            sonar-scanner
                                -Dsonar.projectKey=tp-jenkins
                                -Dsonar.sources=.
                                -Dsonar.tests=tests  // Modifier le chemin des tests si nécessaire
                                -Dsonar.host.url=http://localhost:9000
                                -Dsonar.token=${SONAR_TOKEN}  // Utilisation du token SonarQube récupéré des credentials Jenkins
                        '''
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
