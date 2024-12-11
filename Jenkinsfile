pipeline {
    agent any

    environment {
        // Chemin vers le sonar-scanner installé sur votre machine
        SONAR_SCANNER_HOME = 'C:\\Users\\ADMIN\\OneDrive\\Bureau\\AGIL\\jenkins_home\\sonar-scanner\\bin'  
        // Ajouter sonar-scanner au PATH de l'environnement
        PATH = "${env.PATH};${env.SONAR_SCANNER_HOME}"
        SONAR_TOKEN = credentials('sonar-token2')
        SONAR_HOST_URL = 'http://localhost:9000'
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

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('MySonarQubeServer') {
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


