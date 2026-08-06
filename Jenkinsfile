pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'moeprofit95'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Backend') {
            steps {
                sh 'docker build -t techpathway-backend:v2 ./backend'
            }
        }

        stage('Build Frontend') {
            steps {
                sh 'docker build -t techpathway-frontend:v2 ./frontend'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Tag Images') {
            steps {
                sh '''
                docker tag techpathway-backend:v2 $DOCKERHUB_USERNAME/techpathway-backend:latest
                docker tag techpathway-frontend:v2 $DOCKERHUB_USERNAME/techpathway-frontend:latest
                '''
            }
        }

        stage('Push Backend') {
            steps {
                sh 'docker push $DOCKERHUB_USERNAME/techpathway-backend:latest'
            }
        }

        stage('Push Frontend') {
            steps {
                sh 'docker push $DOCKERHUB_USERNAME/techpathway-frontend:latest'
            }
        }

       stage('Deploy to EC2') {
    steps {
        sshagent(credentials: ['ec2-key']) {
            sh '''
                ssh -o StrictHostKeyChecking=no ubuntu@18.209.13.109 \
                "cd ~/techpathway-2 && \
                sudo docker compose pull && \
                sudo docker compose up -d"
            '''
        }
    }
}