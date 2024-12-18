pipeline {
    agent any

    environment {
        SONAR_SCANNER_PATH = '/opt/sonar-scanner-4.8.0.2856-linux/bin/sonar-scanner' // Mettre à jour avec le bon chemin
        SONAR_TOKEN = credentials('sonartk')  // Remplacer par l'ID de votre credential SonarQube dans Jenkins
        DEPLOY_DIR = 'C:\\xampp\\htdocs\\your_project_folder'  // Répertoire de déploiement XAMPP
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
                    // Exécuter SonarQube sans stopper le pipeline en cas d'échec
                    def sonarAnalysis = sh(script: '''
                        ${SONAR_SCANNER_PATH} \
                            -Dsonar.projectKey=my_project_key \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://sonarqube:9000 \
                            -Dsonar.login=${SONAR_TOKEN}
                    ''', returnStatus: true)

                    // Si l'analyse échoue, afficher un message mais ne pas stopper le pipeline
                    if (sonarAnalysis != 0) {
                        echo 'SonarQube analysis failed, but continuing with the deployment.'
                    }
                }
            }
        }

        stage('Deploy to XAMPP') {
            steps {
                echo 'Deploying to XAMPP...'
                bat '''
                    xcopy /E /I /Y .\\* ${DEPLOY_DIR}\\
                ''' // Cette commande copie tous les fichiers vers le répertoire XAMPP
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
