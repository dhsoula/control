pipeline {
    agent {
        docker {
            image 'php:8.2-cli' // L'image Docker avec PHP
            args '-v /var/run/docker.sock:/var/run/docker.sock' // Monte le socket Docker
        }
    }

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
                    // Installer les dépendances avec Composer
                    sh 'composer install'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    docker.image('sonarsource/sonar-scanner-cli').inside {
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
