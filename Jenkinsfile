pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/somnathsingh712/thedevops.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t node-devops-app .'
            }
        }

        stage('Stop Existing Container') {
            steps {
                sh 'docker stop node-container || true'
                sh 'docker rm node-container || true'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 3000:3000 --name node-container node-devops-app'
            }
        }
    }
}
