pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        DOCKER_USER = 'moeprofit95'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
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

        stage('Docker Login') {
            steps {
                sh '''
                echo "$DOCKERHUB_CREDENTIALS_PSW" | docker login \
                -u "$DOCKERHUB_CREDENTIALS_USR" \
                --password-stdin
                '''
            }
        }

        stage('Tag Images') {
            steps {
                sh '''
                docker tag techpathway-backend:v2 $DOCKER_USER/techpathway-backend:latest
                docker tag techpathway-frontend:v2 $DOCKER_USER/techpathway-frontend:latest
                '''
            }
        }

        stage('Push Backend') {
            steps {
                sh 'docker push $DOCKER_USER/techpathway-backend:latest'
            }
        }

        stage('Push Frontend') {
            steps {
                sh 'docker push $DOCKER_USER/techpathway-frontend:latest'
            }
        }

        stage('List Images') {
            steps {
                sh 'docker images'
            }
        }
    }
}