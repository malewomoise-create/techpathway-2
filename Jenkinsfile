pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Workspace') {
            steps {
                sh 'pwd'
                sh 'ls -la'
            }
        }

        stage('Build Backend Image') {
            steps {
                sh 'docker build -t techpathway-backend:v2 ./backend'
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh 'docker build -t techpathway-frontend:v2 ./frontend'
            }
        }

        stage('List Docker Images') {
            steps {
                sh 'docker images'
            }
        }
    }
}