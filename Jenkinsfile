pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                git 'YOUR_GITHUB_REPO'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t node-devops-app .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker stop node-container || true'
                sh 'docker rm node-container || true'
                sh 'docker run -d -p 3000:3000 --name node-container node-devops-app'
            }
        }
    }
}
