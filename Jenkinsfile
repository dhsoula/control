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
                    // Utilisation de Docker pour exécuter SonarScanner
                    sh """
                        docker run --rm \
                            -e SONAR_HOST_URL=http://localhost:9000 \
                            -e SONAR_LOGIN=${SONAR_TOKEN} \
                            -v ${PWD}:/usr/src \
                            sonarsource/sonar-scanner-cli \
                            -Dsonar.projectKey=tp-jenkinse \
                            -Dsonar.sources=. \
                            -Dsonar.tests=tests
                    """
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
