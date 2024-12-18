pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://your-sonarqube-url:9000'  // Replace with your SonarQube URL
        SONAR_TOKEN = credentials('sonartk')          // Use Jenkins credentials for security
        SONAR_SCANNER_PATH = 'sonar-scanner-dir/sonar-scanner-4.8.0.2856-linux/bin/sonar-scanner'
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

        stage('Setup SonarScanner') {
            steps {
                script {
                    echo 'Downloading SonarScanner locally...'
                    sh '''
                        # Download SonarScanner CLI to Jenkins workspace
                        curl -o sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.8.0.2856-linux.zip
                        
                        # Unzip SonarScanner
                        unzip sonar-scanner.zip -d sonar-scanner-dir
                        
                        # Make the SonarScanner executable
                        chmod +x sonar-scanner-dir/sonar-scanner-4.8.0.2856-linux/bin/sonar-scanner
                    '''
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Performing SonarQube analysis...'
                sh '''
                    # Run SonarScanner from workspace
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
