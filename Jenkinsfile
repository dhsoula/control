pipeline {
    agent any
    
    environment {
        SONAR_TOKEN = credentials('sonar_token')
        SONAR_HOST_URL = 'http://localhost:9000' // URL du serveur SonarQube
    }
    
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                script {
                    sh 'composer install --no-interaction --prefer-dist'
                }
            }
        }
        
        stage('Run Tests') {
            steps {
                script {
                    sh 'chmod +x vendor/bin/phpunit'
                    sh 'vendor/bin/phpunit --config phpunit.xml'
                }
            }
        }
    }
    stage('SonarQube Analysis') {
    steps {
        script {
            withSonarQubeEnv('MySonarQubeServer') {
                sh 'mvn clean verify sonar:sonar'
            }
        }
    }
}

    
    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

