pipeline {
    agent any
    
    tools {
        // Utilisation du bon nom de l'outil configuré dans Jenkins
        sonarQubeScanner 'SonarQubeScanner'  // Nom correct de l'outil configuré
    }
    
    environment {
        SONAR_TOKEN = credentials('sonar_token')
        SONAR_HOST_URL = 'http://localhost:9000' // URL du serveur SonarQube
    }
    
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                script {
                    sh 'composer install --no-interaction --prefer-dist'
                }
            }
        }
        
        stage('Run Tests') {
            steps {
                script {
                    sh 'chmod +x vendor/bin/phpunit'
                    sh 'vendor/bin/phpunit --config phpunit.xml'
                }
            }
        }
        
        stage('Code Analysis') {
            steps {
                script {
                    // Exécuter SonarQube Scanner avec les paramètres nécessaires
                    withSonarQubeEnv('SonarQube') {  // Utilise l'environnement SonarQube configuré
                        sh """
                            sonar-scanner \
                            -Dsonar.projectKey=tp-jenkinse \
                            -Dsonar.sources=. \
                            -Dsonar.tests=tests
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
