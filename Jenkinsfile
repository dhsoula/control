pipeline {
    agent any

    environment {
        SONAR_SCANNER_PATH = '/opt/sonar-scanner-4.8.0.2856-linux/bin/sonar-scanner' // Mettre à jour avec le bon chemin
        SONAR_TOKEN = credentials('sonartk')  // Remplacer par l'ID de votre credential SonarQube dans Jenkins
    }

    stages {

        stage('Checkout SCM') {
            steps {
                echo 'Checking out source code from SCM...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing project dependencies...'
                sh '''
                    composer install --no-interaction --prefer-dist
                '''
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running PHPUnit tests...'
                sh '''
                    chmod +x vendor/bin/phpunit  # Make PHPUnit executable
                    vendor/bin/phpunit --configuration phpunit.xml
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Performing SonarQube analysis...'
                withSonarQubeEnv('SonarQube') {  // Remplacer 'SonarQube' par la configuration de votre serveur SonarQube dans Jenkins
                    sh '''
                        ${SONAR_SCANNER_PATH} \
                            -Dsonar.projectKey=my_project_key \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://sonarqube:9000 \
                            -Dsonar.login=${SONAR_TOKEN}
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}

