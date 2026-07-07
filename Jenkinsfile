pipeline {
    agent any
 
    environment {
        IMAGE_NAME = "task-tracker"
        IMAGE_TAG = "latest"
    }
 
    stages {
 
        stage('SCM Pull') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/gururajmn/Intelli-Assigment.git'
            }
        }
 
        stage('Install Dependencies and Run Tests') {
            steps {
                sh '''
                    npm install
                    npm test
                '''
            }
        }
 
        stage('Build') {
            steps {
                sh '''
                    docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                '''
            }
        }
 
        stage('Deploy') {
            steps {
                sh '''
                    docker rm -f task-tracker || true
                    docker-compose up -d
                '''
            }
        }
 
        stage('Wait for Application') {
            steps {
                sh '''
                    echo "Waiting for application..."
                    sleep 10
                '''
            }
        }
 
        stage('Curl') {
            steps {
                sh '''
                    echo "========== HOME =========="
                    curl --retry 10 --retry-delay 2 --retry-connrefused http://localhost:3000/
 
                    echo ""
                    echo "========== HEALTH =========="
                    curl --retry 10 --retry-delay 2 --retry-connrefused http://localhost:3000/health
 
                    echo ""
                    echo "========== TASKS =========="
                    curl --retry 10 --retry-delay 2 --retry-connrefused http://localhost:3000/api/tasks
                '''
            }
        }
 
        stage('Cleanup') {
            steps {
                sh '''
                    docker image prune -f
                '''
                cleanWs()
            }
        }
    }
}
