pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token2')
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_SCANNER_HOME = 'C:\\sonar-scanner-6.2.1.4610-windows-x64'  // Pour Windows
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

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('MySonarQubeServer') {
                        bat """
                        C:\\sonar-scanner-6.2.1.4610-windows-x64\\bin\\sonar-scanner.bat ^
                        -Dsonar.projectKey=tp-jenkinse ^
                        -Dsonar.sources=./ ^
                        -Dsonar.host.url=${SONAR_HOST_URL} ^
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
