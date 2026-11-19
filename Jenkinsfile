
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Express source code checked out by Jenkins'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    npm install
                '''
            }
        }

        stage('Deploy Express') {
            steps {
                echo 'Express deployment stage'
            }
        }
    }
}
