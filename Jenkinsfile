pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token2')  // Get the SonarQube token from Jenkins credentials
        SONAR_HOST_URL = 'http://localhost:9000'   // SonarQube server URL
        SONAR_SCANNER_HOME = 'C:\\sonar-scanner-6.2.1.4610-windows-x64'  // Path to SonarQube Scanner on Windows
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
                    bat 'composer install --no-interaction --prefer-dist'  // Use bat instead of sh for Windows
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    bat 'chmod +x vendor/bin/phpunit'  // Make the PHPUnit script executable (optional in Windows, can be removed)
                    bat 'vendor/bin/phpunit --config phpunit.xml'  // Run PHPUnit tests
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('MySonarQubeServer') {  // Ensure 'MySonarQubeServer' is correctly configured in Jenkins
                        bat """
                        C:\\sonar-scanner-6.2.1.4610-windows-x64\\bin\\sonar-scanner.bat ^
                        -Dsonar.projectKey=tp-jenkinse ^
                        -Dsonar.sources=./ ^
                        -Dsonar.host.url=${SONAR_HOST_URL} ^
                        -Dsonar.login=${SONAR_TOKEN}
                        """  // Run SonarQube analysis with the full path to the sonar-scanner.bat file
                    }
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

