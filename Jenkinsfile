pipeline {
    agent any  // Run on any available agent

    environment {
        SONAR_TOKEN = credentials('sonar_token')
        SONAR_HOST_URL = 'http://localhost:9000'
        GITBASH_PATH = 'C:\\Program Files\\Git\\bin\\bash.exe'  // Path to Git Bash (for Windows)
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout scm  // Clone the repository
                }
            }
        }

        stage('Install Composer') {
            steps {
                script {
                    echo "Installing Composer"
                    sh 'curl -sS https://getcomposer.org/installer | php'  // Install Composer
                    sh 'mv composer.phar /usr/local/bin/composer'        // Move it to PATH
                    sh 'export PATH=$PATH:/usr/local/bin'                 // Ensure it's in PATH
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    echo "Installing PHP dependencies with Composer"
                    sh 'composer install'  // Now Composer should work
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    echo "Running SonarQube analysis"
                    sh "sonar-scanner -Dsonar.projectKey=control -Dsonar.sources=. -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.login=$SONAR_TOKEN"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying the application"
                    sh 'mkdir -p /path/to/production/folder && cp -r * /path/to/production/folder'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for more details.'
        }
    }
}

