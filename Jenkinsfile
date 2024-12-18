pipeline {
    agent any

    environment {
        SONAR_SCANNER_PATH = '/opt/sonar-scanner-4.8.0.2856-linux/bin/sonar-scanner' // Update with the correct installed path
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
                withSonarQubeEnv('SonarQube') {  // Replace 'SonarQube' with your Jenkins SonarQube server config
                    sh '''
                        ${SONAR_SCANNER_PATH} \
                            -Dsonar.projectKey=my_project_key \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://your-sonar-server:9000 \
                            -Dsonar.login=${SONAR_TOKEN}
                    '''
                }
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


