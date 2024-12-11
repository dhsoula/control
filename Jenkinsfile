pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar_token') // Assure-toi que le token est bien configuré
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

        stage('Test Docker') {
            steps {
                script {
                    // Exécute un conteneur simple pour tester Docker
                    sh 'docker run --rm hello-world'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Utilisation de l'environnement SonarQube configuré dans Jenkins
                    withSonarQubeEnv('MySonarQubeServer') {
                        // L'outil de build ici pourrait être Maven ou un autre (exemple avec Maven)
                        sh 'mvn clean verify sonar:sonar'
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
