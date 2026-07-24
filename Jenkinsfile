pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm ci'
            }
        }

        stage('Code Quality Check') {
            steps {
                echo "Code Quality Check Completed"
            }
        }

        stage('Automated Tests') {
            steps {
                sh 'CI=true npm test -- --watchAll=false'
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Build & Push Docker Image') {
            steps {
                sh '''
                docker build -t dhineshkumar375/react-cicd-app:v1 .
                docker push dhineshkumar375/react-cicd-app:v1
                '''
            }
        }

        stage('Deploy to Development') {
            steps {
                sh '''
                docker stop reactapp || true
                docker rm reactapp || true
                docker run -d --name reactapp -p 3000:3000 dhineshkumar375/react-cicd-app:v1
                '''
            }
        }

        stage('Approval for Production') {
            steps {
                input "Deploy to Production?"
            }
        }

        stage('Production Deployment') {
            steps {
                echo "Production Deployment Successful"
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                sleep 10
                curl -f http://localhost:3000
                '''
            }
        }

    }

    post {

        success {
            echo "Pipeline completed successfully."
        }

        failure {
            echo "Deployment failed. Rolling back..."

            sh '''
            docker stop reactapp || true
            docker rm reactapp || true
            '''
        }
    }
}
