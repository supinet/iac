pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building backend...'
                script {
                    docker.image('node:22-alpine').inside('--entrypoint ""') {
                        sh 'npm install'
                        sh 'npm run build'
                    }
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing backend...'
                 script {
                    docker.image('node:22-alpine').inside('--entrypoint ""') {
                        sh 'npm test'
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}