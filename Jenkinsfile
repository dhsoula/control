pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token2')  // Get the SonarQube token from Jenkins credentials
        SONAR_HOST_URL = 'http://localhost:9000'   // SonarQube server URL
        SCANNER_HOME = 'C:\\Users\\ADMIN\\OneDrive\\Bureau\\AGIL\\jenkins_home\\plugins\\sonar-scanner\\bin\\sonar-scanner.bat'
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
                 withSonarQubeEnv('MySonarQubeServer') {
                    sh "${SCANNER_HOME} -Dsonar.projectName=tp -Dsonar.java.binaries=. -Dsonar.projectKey=tp-jenkins"
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
