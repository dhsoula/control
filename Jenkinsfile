pipeline {
    agent any
    
    tools {
        // Utilisation du bon nom de l'outil configuré dans Jenkins
        sonarQube 'SonarQube'  // Nom de l'outil SonarQube configuré dans Jenkins
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
        
        stage('Code Linting with PHP_CodeSniffer') {
            steps {
                script {
                    // Installer PHP_CodeSniffer via Composer si nécessaire
                    sh 'composer require --dev squizlabs/php_codesniffer'
                    // Lancer l'analyse de code avec PHP_CodeSniffer
                    sh 'vendor/bin/phpcs --standard=PSR2 src/ tests/'
                }
            }
        }
        
        stage('Code Analysis with SonarQube') {
            steps {
                script {
                    // Exécuter SonarQube pour l'analyse de code
                    withSonarQubeEnv('SonarQube') {  // Utilisation de l'environnement SonarQube configuré
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
