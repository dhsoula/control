pipeline {
    agent { label 'linux' }

    environment {
        SONAR_TOKEN = credentials('sonar_token')
        SONAR_HOST_URL = 'http://localhost:9000'
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Cloning repository..."
                git 'https://github.com/dhsoula/control.git'
            }
        }

        stage('Build') {
            steps {
                echo "Running Composer install..."
                sh 'composer install || echo "Composer install failed!"'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "Starting SonarQube analysis..."
                sh '''
                sonar-scanner \
                -Dsonar.projectKey=control \
                -Dsonar.sources=. \
                -Dsonar.host.url=$SONAR_HOST_URL \
                -Dsonar.login=$SONAR_TOKEN || echo "Sonar analysis failed!"
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying application..."
                sh '''
                mkdir -p /path/to/production/folder
                cp -r * /path/to/production/folder || echo "Deploy failed!"
                '''
            }
        }
    }
}
