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

        stage('Install Dependencies') {
            steps {
                script {
                    echo "Installing PHP dependencies with Composer"
                    if (isUnix()) {
                        sh 'composer install'  // For Linux or Unix-like agents
                    } else {
                        bat "\"${env.GITBASH_PATH}\" -c 'composer install'"  // For Windows agents
                    }
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    echo "Running SonarQube analysis"
                    if (isUnix()) {
                        sh "sonar-scanner -Dsonar.projectKey=control -Dsonar.sources=. -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.login=$SONAR_TOKEN"
                    } else {
                        bat "\"${env.GITBASH_PATH}\" -c 'sonar-scanner -Dsonar.projectKey=control -Dsonar.sources=. -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.login=$SONAR_TOKEN'"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying the application"
                    if (isUnix()) {
                        sh 'mkdir -p /path/to/production/folder && cp -r * /path/to/production/folder'
                    } else {
                        bat "\"${env.GITBASH_PATH}\" -c 'mkdir -p C:/path/to/production/folder && xcopy /e /i /h * C:/path/to/production/folder'"
                    }
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

