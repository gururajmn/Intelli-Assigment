pipeline {

    agent any

    environment {
        APP_NAME = "task-tracker"
        IMAGE_NAME = "task-tracker"
        APP_PORT = "3000"
    }

    stages {

        stage('SCM Pull') {
            steps {
                checkout scm
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
                    docker build -t ${IMAGE_NAME}:latest .
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker compose down || true
                    docker compose up -d
                '''
            }
        }

        stage('Curl Verification') {
            steps {
                sh '''
                    sleep 10

                    curl http://localhost:3000/

                    curl http://localhost:3000/health

                    curl http://localhost:3000/api/tasks
                '''
            }
        }
    }

    post {
        always {
            sh '''
                docker compose down || true
                docker image prune -f
            '''

            cleanWs()
        }
    }
}
