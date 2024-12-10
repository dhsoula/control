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
                    // Installer Composer avec Git Bash
                    bat '"C:\\Program Files\\Git\\bin\\bash.exe" -c "curl -sS https://getcomposer.org/installer | php"'
                    bat '"C:\\Program Files\\Git\\bin\\bash.exe" -c "mv composer.phar /usr/local/bin/composer"'
                    bat '"C:\\Program Files\\Git\\bin\\bash.exe" -c "export PATH=$PATH:/usr/local/bin"'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    echo "Installing PHP dependencies with Composer"
                    // Installer les dépendances PHP avec Composer
                    bat '"C:\\Program Files\\Git\\bin\\bash.exe" -c "composer install"'
                }
            }
        }

        stage('Phing Build') {
            steps {
                script {
                    echo "Running Phing Build"
                    // Utiliser Phing pour effectuer le build (assurez-vous que Phing est installé)
                    bat '"C:\\Program Files\\Git\\bin\\bash.exe" -c "phing build"'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    echo "Running SonarQube analysis"
                    // Lancer l'analyse SonarQube avec Sonar Scanner
                    bat '"C:\\Program Files\\Git\\bin\\bash.exe" -c "sonar-scanner -Dsonar.projectKey=control -Dsonar.sources=. -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.login=$SONAR_TOKEN"'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying the application"
                    // Copier les fichiers vers le dossier de production
                    bat '"C:\\Program Files\\Git\\bin\\bash.exe" -c "mkdir -p /path/to/production/folder && cp -r * /path/to/production/folder"'
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


