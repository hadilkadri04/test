pipeline {
    agent any

    // Optionnel : si tu as configuré NodeJS dans Jenkins Global Tools, décommente et mets le bon nom
    // tools { nodejs 'NodeJS_16' }

    stages {
        stage('Checkout') {
            steps {
                echo 'Récupération du code depuis Git...'
                // checkout scm nécessite que ton job soit "Pipeline script from SCM" ou Multibranch
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installation des dépendances Node.js...'
                script {
                    if (isUnix()) {
                        sh 'npm install'
                    } else {
                        // utilisation de bat sur Windows
                        bat 'npm install'
                    }
                }
            }
        }

        stage('Tests') {
            steps {
                echo 'Exécution des tests...'
                script {
                    if (isUnix()) {
                        sh 'npm test'
                    } else {
                        bat 'npm test'
                    }
                }
            }
        }

        stage('Build Docker') {
            steps {
                echo "Construction de l'image Docker..."
                script {
                    if (isUnix()) {
                        sh "docker build -t todo-app ."
                    } else {
                        // Sur Windows, si Docker Desktop est installé et docker accessible, bat fonctionnera
                        bat 'docker build -t todo-app .'
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline exécuté avec succès !'
        }
        failure {
            echo 'Le pipeline a échoué.'
        }
    }
}
