pipeline {
    agent any

    environment {
        SONAR_SCANNER_PATH = '/opt/sonar-scanner-4.8.0.2856-linux/bin/sonar-scanner' // Update with the correct path
        SONAR_TOKEN = credentials('sonartk')  // Replace with your SonarQube credential ID in Jenkins
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
                    chmod +x vendor/bin/phpunit  # Make PHPUnit executable
                    vendor/bin/phpunit --configuration phpunit.xml
                '''
            }
        }

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
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh '''
                    docker build -t my_project_image .
                '''
            }
        }

        stage('Deploy with Docker') {
            steps {
                echo 'Deploying application using Docker...'
                sh '''
                    docker stop my_project_container || true
                    docker rm my_project_container || true
                    docker run -d --name my_project_container -p 8080:80 my_project_image
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

