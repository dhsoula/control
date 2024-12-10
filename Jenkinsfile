pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar_token')  // Token SonarQube
        SONAR_HOST_URL = 'http://localhost:9000'  // URL de SonarQube
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/dhsoula/control.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    // Utiliser Composer installé localement
                    bat 'composer install'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    sh '''
                    sonar-scanner \
                    -Dsonar.projectKey=control \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=$SONAR_HOST_URL \
                    -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Déploiement (par exemple : copier les fichiers)
                    sh '''
                    mkdir -p /path/to/production/folder
                    cp -r * /path/to/production/folder
                    '''
                }
            }
        }
    }
}


