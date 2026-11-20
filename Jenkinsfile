
pipeline {
    agent any

    options {
        timestamps()
        timeout(time: 15, unit: 'MINUTES')
    }

    environment {
        APP_DIR = '/home/ubuntu/projects/express-frontend'
        API_URL = 'http://127.0.0.1:5000/api'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Validate Project') {
            steps {
                sh '''
                    test -f app.js
                    test -f package.json
                    test -f package-lock.json
                    echo "Express project files found"
                '''
            }
        }

        stage('Deploy Latest Code') {
            steps {
                sh '''
                    rsync -rD --no-owner --no-group --no-perms --omit-dir-times --delete \
                      --exclude='.git/' \
                      --exclude='node_modules/' \
                      "$WORKSPACE/" "$APP_DIR/"
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                dir('/home/ubuntu/projects/express-frontend') {
                    sh 'npm ci'
                }
            }
        }

        stage('Restart Express') {
            steps {
                sh '''
                    sudo -n -u ubuntu /usr/local/bin/pm2 restart express-frontend --update-env
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                    curl --fail --silent --show-error \
                      --retry 5 --retry-delay 2 \
                      http://127.0.0.1:3000/
                '''
            }
        }
    }

    post {
        success {
            echo 'Express frontend deployed successfully!'
        }

        failure {
            echo 'Express deployment failed. Check the console output.'
        }
    }
}
