pipeline {
    agent { label 'linux' }  // Exécuter sur un nœud Linux

    environment {
        SONAR_TOKEN = credentials('sonar_token')
        SONAR_HOST_URL = 'http://localhost:9000'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/dhsoula/control.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'composer install'  // Utiliser sh sur un nœud Linux
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    sh '''
                    sonar-scanner \
                    -Dsonar.projectKey=control \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=$SONAR_HOST_URL \
                    -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh '''
                    mkdir -p /path/to/production/folder
                    cp -r * /path/to/production/folder
                    '''
                }
            }
        }
    }
}



