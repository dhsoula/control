pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonartk')  // SonarQube token
        SONAR_HOST_URL = 'http://localhost:9000'  // SonarQube server URL
    }

    tools {
        sonarQube 'sonar-scanner'  // Ensure this matches the name in your Jenkins Global Tool Configuration
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm  // Checkout the source code from SCM
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'composer install --no-interaction --prefer-dist'  // For Unix-based systems
                    } else {
                        bat 'composer install --no-interaction --prefer-dist'  // For Windows systems
                    }
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'chmod +x vendor/bin/phpunit'  // Make the PHPUnit script executable
                    }
                    sh 'vendor/bin/phpunit --configuration phpunit.xml'  // Run PHPUnit tests
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('MySonarQubeServer') {
                    sh """
                        sonar-scanner \
                            -Dsonar.projectKey=tp \
                            -Dsonar.sources=src \
                            -Dsonar.host.url=${SONAR_HOST_URL} \
                            -Dsonar.login=${SONAR_TOKEN}
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'  // Display success message
        }
        failure {
            echo 'Pipeline failed.'  // Display failure message
        }
    }
}

