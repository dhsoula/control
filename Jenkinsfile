pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token2')  // Récupère le jeton SonarQube depuis les Credentials de Jenkins
        SONAR_HOST_URL = 'http://localhost:9000'   // URL du serveur SonarQube
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm  // Récupère le code source depuis Git
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Installe les dépendances PHP via Composer
                    sh 'composer install --no-interaction --prefer-dist'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Rendre phpunit exécutable et exécuter les tests
                    sh 'chmod +x vendor/bin/phpunit'
                    sh 'vendor/bin/phpunit --config phpunit.xml'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Utilisation de l'environnement SonarQube configuré dans Jenkins
                    withSonarQubeEnv('MySonarQubeServer') {
                        // Lancer l'analyse avec le scanner SonarQube et le projet spécifique
                        sh """
                        sonar-scanner \
                        -Dsonar.projectKey=tp-jenkinse \
                        -Dsonar.sources=./ \
                        -Dsonar.host.url=${SONAR_HOST_URL} \
                        -Dsonar.login=${SONAR_TOKEN}
                        """
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
