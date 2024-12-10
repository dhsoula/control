pipeline {
    agent any
    
    environment {
        SONAR_TOKEN = credentials('sonar_token') // Ajoutez votre token SonarQube ici
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
                    // Commande de build de votre projet PHP, par exemple :
                    sh 'composer install'
                }
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Commande SonarQube pour analyser votre code
                    sh 'mvn sonar:sonar -Dsonar.projectKey=control -Dsonar.host.url=http://localhost:9000 -Dsonar.login=$SONAR_TOKEN'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    // Déploiement dans le dossier de simulation de production
                    sh 'cp -R * /path/to/production/folder'
                }
            }
        }
    }
}
