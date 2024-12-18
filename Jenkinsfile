pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://your-sonarqube-url:9000'  // Replace with your SonarQube URL
        SONAR_TOKEN = credentials('sonartk')          // Jenkins credentials for SonarQube token
        SONAR_SCANNER_PATH = '/opt/sonar-scanner-4.8.0.2856-linux/bin/sonar-scanner' // Update with the correct installed path
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
                    chmod +x vendor/bin/phpunit  // Make PHPUnit executable
                    vendor/bin/phpunit --configuration phpunit.xml
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Performing SonarQube analysis...'
                sh '''
                    chmod +x ${SONAR_SCANNER_PATH}  // Make SonarScanner executable
                    ${SONAR_SCANNER_PATH} \
                        -Dsonar.projectKey=tp \
                        -Dsonar.sources=src \
                        -Dsonar.host.url=${SONAR_HOST_URL} \
                        -Dsonar.login=${SONAR_TOKEN}
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}

