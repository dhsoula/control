pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar_token')
        SONAR_HOST_URL = 'http://localhost:9000'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: '5f144011-1480-4623-ae64-dc4d512d9045', branch: 'master', url: 'https://github.com/dhsoula/control.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    // Exécute la commande avec Git Bash en utilisant son chemin complet
                    sh '"C:/Program Files/Git/bin/bash.exe" -c "composer install"'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Exécute l'analyse SonarQube avec Git Bash
                    sh '"C:/Program Files/Git/bin/bash.exe" -c "sonar-scanner -Dsonar.projectKey=control -Dsonar.sources=. -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.login=$SONAR_TOKEN"'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Déployer les fichiers avec Git Bash
                    sh '"C:/Program Files/Git/bin/bash.exe" -c "mkdir -p C:/path/to/production/folder && xcopy /E /I * C:/path/to/production/folder"'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline exécutée avec succès !'
        }
        failure {
            echo 'Pipeline échouée. Vérifiez les logs pour plus de détails.'
        }
    }
}
