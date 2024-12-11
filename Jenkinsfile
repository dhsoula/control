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
                        sh 'chmod +x vendor/bin/phpunit'  // Make the PHPUnit script executable on Unix-based systems
                    }
                    sh 'vendor/bin/phpunit --config phpunit.xml'  // Run PHPUnit tests
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('MySonarQubeServer') {  // Ensure 'MySonarQubeServer' is correctly configured in Jenkins
                        if (isUnix()) {
                            // Use sh for Unix-based systems
                            sh """
                            sonar-scanner \
                            -Dsonar.projectKey=tp-jenkinse \
                            -Dsonar.sources=./ \
                            -Dsonar.host.url=${SONAR_HOST_URL} \
                            -Dsonar.login=${SONAR_TOKEN}
                            """
                        } else {
                            // Use bat for Windows
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
