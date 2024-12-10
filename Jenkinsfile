pipeline { 
    agent { label 'windows' }  // Ensure it runs on a Windows node

    environment {
        SONAR_TOKEN = credentials('sonar_token')
        SONAR_HOST_URL = 'http://localhost:9000'
        GITBASH_PATH = 'C:\\Program Files\\Git\\bin\\bash.exe'  // Path to Git Bash
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout scm  // Clone the repository
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    echo "Installing PHP dependencies with Composer"
                    bat "\"${env.GITBASH_PATH}\" -c 'composer install'"  // Using Git Bash to run Composer
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    echo "Running SonarQube analysis"
                    bat "\"${env.GITBASH_PATH}\" -c 'sonar-scanner -Dsonar.projectKey=control -Dsonar.sources=. -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.login=$SONAR_TOKEN'"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying the application"
                    bat "\"${env.GITBASH_PATH}\" -c 'mkdir -p C:/path/to/production/folder && xcopy /e /i /h * C:/path/to/production/folder'"
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

