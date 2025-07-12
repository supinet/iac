pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building backend...'
                cd backend
                npm build
            }
        }
        stage('Test') {
            steps {
                echo 'Testing backend...'
                npm run test
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}