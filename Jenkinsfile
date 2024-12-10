pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar_token')
        SONAR_HOST_URL = 'http://localhost:9000'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'votre_identifiant_jenkins', branch: 'master', url: 'https://github.com/dhsoula/control.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    bat 'composer install'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    bat """
                    sonar-scanner ^
                    -Dsonar.projectKey=control ^
                    -Dsonar.sources=. ^
                    -Dsonar.host.url=%SONAR_HOST_URL% ^
                    -Dsonar.login=%SONAR_TOKEN%
                    """
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    bat """
                    mkdir C:\\path\\to\\production\\folder
                    xcopy /E /I * C:\\path\\to\\production\\folder
                    """
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

