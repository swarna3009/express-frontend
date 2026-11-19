
pipeline {
    agent any

    environment {
        BACKEND_URL = 'http://52.66.47.58:5000/api'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out Express frontend'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Check Flask Backend API') {
            steps {
                sh '''
                    echo "Checking Flask API..."
                    curl --fail --show-error --silent "$BACKEND_URL"
                '''
            }
        }

        stage('Deploy Express') {
            steps {
                echo 'Express frontend deployment stage'
            }
        }
    }
}
