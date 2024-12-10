pipeline {
    agent any

    environment {
        SONARQUBE = 'SonarQube'  // Nom de votre configuration SonarQube dans Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                // Récupérer le code depuis le repository
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                script {
                    if (isUnix()) {
                        // Installation des dépendances avec Composer (Linux/Mac)
                        sh 'composer install'
                    } else {
                        // Installation des dépendances avec Composer (Windows)
                        bat 'composer install'
                    }
                }
            }
        }

        stage('Code Quality Analysis') {
            steps {
                script {
                    // Assurez-vous que vous avez configuré SonarQube dans Jenkins
                    withSonarQubeEnv(SONARQUBE) {
                        if (isUnix()) {
                            // Exécution de l'analyse SonarQube sur un environnement Unix/Linux
                            sh 'vendor/bin/sonar-scanner'
                        } else {
                            // Exécution de l'analyse SonarQube sur un environnement Windows
                            bat 'vendor\\bin\\sonar-scanner'
                        }
                    }
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    if (isUnix()) {
                        // Si vous avez un processus de build spécifique (ex: génération de fichiers de cache)
                        // Cela peut être nécessaire pour certains projets PHP (par exemple, la compilation d'assets)
                        sh 'php artisan optimize' // Exemple pour Laravel, remplacez par votre commande si nécessaire
                    } else {
                        // Commande de build spécifique pour Windows
                        bat 'php artisan optimize' // Exemple pour Laravel
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Déploiement, selon l'environnement
                    if (isUnix()) {
                        // Déploiement sur Unix/Linux (par exemple, via SSH ou autre méthode)
                        sh './deploy.sh'
                    } else {
                        // Déploiement sur Windows (par exemple, via un script batch)
                        bat 'deploy.bat'
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Build et analyse SonarQube réussis!'
        }
        failure {
            echo 'Le build ou l\'analyse SonarQube a échoué.'
        }
    }
}

