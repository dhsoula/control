pipeline {
    agent any
    environment {
        SONAR_TOKEN = credentials('sonar_token')
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
        stage('Code Analysis') {
            steps {
                script {
                    sh 'C:\\sonar-scanner-6.2.1.4610-windows-x64\\bin\\sonar-scanner.bat -Dsonar.projectKey=tp-jenkins -Dsonar.sources=. -Dsonar.tests=tests -Dsonar.host.url=http://localhost:9000 -Dsonar.token=${SONAR_TOKEN}'
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
