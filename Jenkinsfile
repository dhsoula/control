pipeline {
    agent any

    environment {
        SONAR_SCANNER_PATH = '/opt/sonar-scanner-4.8.0.2856-linux/bin/sonar-scanner'  // Correct the path if needed
        SONAR_TOKEN = credentials('sonartk')  // Replace with your SonarQube credential ID in Jenkins
    }

    stages {
        // Checkout source code
        stage('Checkout SCM') {
            steps {
                echo 'Checking out source code from SCM...'
                checkout scm
            }
        }

        // Install project dependencies (using Composer for PHP)
        stage('Install Dependencies') {
            steps {
                echo 'Installing project dependencies...'
                sh '''
                    composer install --no-interaction --prefer-dist
                '''
            }
        }

        // Run PHPUnit tests
        stage('Run Tests') {
            steps {
                echo 'Running PHPUnit tests...'
                sh '''
                    chmod +x vendor/bin/phpunit  # Make PHPUnit executable
                    vendor/bin/phpunit --configuration phpunit.xml
                '''
            }
        }

        // Perform SonarQube analysis
        stage('SonarQube Analysis') {
            steps {
                echo 'Performing SonarQube analysis...'
                script {
                    // Run SonarQube analysis without stopping the pipeline on failure
                    def sonarAnalysis = sh(script: '''
                        ${SONAR_SCANNER_PATH} \
                            -Dsonar.projectKey=my_project_key \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://sonarqube:9000 \
                            -Dsonar.login=${SONAR_TOKEN}
                    ''', returnStatus: true)
                    
                    // Check the status of SonarQube analysis and log if failed
                    if (sonarAnalysis != 0) {
                        echo 'SonarQube analysis failed, but continuing the pipeline.'
                    }
                }
            }
        }

        // Build Docker image
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh '''
                    docker build -t my_project_image .
                '''
            }
        }

        // Deploy with Docker
        stage('Deploy with Docker') {
            steps {
                echo 'Deploying application using Docker...'
                sh '''
                    docker stop my_project_container || true  # Stop container if it exists
                    docker rm my_project_container || true    # Remove container if it exists
                    docker run -d --name my_project_container -p 8080:80 my_project_image  # Run container
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
