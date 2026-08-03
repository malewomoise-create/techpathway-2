pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Docker') {
            steps {
                sh 'docker --version'
            }
        }

        stage('Build Backend Image') {
            steps {
                sh 'docker build -t techpathway-backend:v1 ./backend'
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh 'docker build -t techpathway-frontend:v1 ./frontend'
            }
        }

        stage('List Docker Images') {
            steps {
                sh 'docker images'
            }
        }
    }
}