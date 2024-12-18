pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonartk')  // SonarQube token
        SONAR_HOST_URL = 'http://localhost:9000'  // SonarQube server URL
        SONAR_SCANNER_PATH = '/opt/sonar-scanner-4.8.0.2856-linux/bin/sonar-scanner'  // Correct path to Sonar Scanner
    }

    stages {
        stage('Checkout SCM') {
            steps {
                echo 'Checking out source code from SCM...'
                checkout scm  // Checkout the source code from SCM
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Installing project dependencies...'
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
                    echo 'Running PHPUnit tests...'
                    if (isUnix()) {
                        sh 'chmod +x vendor/bin/phpunit'  // Make PHPUnit executable
                    }
                    sh 'vendor/bin/phpunit --configuration phpunit.xml'  // Run PHPUnit tests
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    echo 'Performing SonarQube analysis...'
                    sh """
                        chmod +x ${SONAR_SCANNER_PATH}  // Ensure sonar-scanner is executable
                        ${SONAR_SCANNER_PATH} \
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
            echo 'Pipeline completed successfully.'  // Success message
        }
        failure {
            echo 'Pipeline failed.'  // Failure message
        }
    }
}
