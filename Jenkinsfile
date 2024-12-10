pipeline {
    agent any
    tools {
        // Vous pouvez spécifier les outils nécessaires, comme NodeJS ou PHP si nécessaire
        // nodejs 'NodeJS'  // Si vous avez besoin de NodeJS pour d'autres étapes
    }
    environment {
        // Récupération du token SonarQube à partir des credentials Jenkins
        SONAR_TOKEN = credentials('sonar_token')  // Utilisation du secret 'sonar_token' stocké dans Jenkins
    }
    stages {
        // Etape pour cloner le dépôt depuis GitHub
        stage('Checkout SCM') {
            steps {
                // Utilisation de l'ID des credentials Git pour le clonage
                checkout scm
            }
        }
        
        // Etape pour installer les dépendances (si nécessaire, vous pouvez adapter cette étape selon vos besoins PHP)
        stage('Install Dependencies') {
            steps {
                script {
                    // Par exemple, vous pouvez installer Composer si nécessaire pour PHP
                    // sh 'composer install'   // Décommentez si vous utilisez Composer dans votre projet PHP
                    echo 'Aucune dépendance spécifique à installer pour ce projet PHP'
                }
            }
        }
        
        // Etape pour exécuter les tests (si vous avez des tests PHP à exécuter)
        stage('Run Tests') {
            steps {
                script {
                    // Si vous avez des tests unitaires PHP à exécuter, utilisez la commande PHPUnit
                    // sh 'phpunit --config phpunit.xml'  // Exemple de commande PHPUnit
                    echo 'Aucun test PHP spécifique à exécuter'
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
                            sonar-scanner \
                                -Dsonar.projectKey=tp-jenkins \
                                -Dsonar.sources=. \  // Pointing to the root directory
                                -Dsonar.tests=test \  // Including the 'test' folder for tests
                                -Dsonar.host.url=http://localhost:9000 \
                                -Dsonar.token=${SONAR_TOKEN}  // Utilisation du token SonarQube récupéré des credentials Jenkins
                        '''
                    }
                }
            }
        }
    }
}
