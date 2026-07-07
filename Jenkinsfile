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
                    docker rm -f task-tracker || true
                    docker compose down || true
                    docker compose up -d --build
                '''
            }
        }
 
        stage('Curl Verification') {
            steps {
                sh '''
                    sleep 10
 
                    echo "========== ROOT =========="
                    curl http://localhost:3000/
 
                    echo ""
 
                    echo "========== HEALTH =========="
                    curl http://localhost:3000/health
 
                    echo ""
 
                    echo "========== TASKS =========="
                    curl http://localhost:3000/api/tasks
 
                    echo ""
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
➕

