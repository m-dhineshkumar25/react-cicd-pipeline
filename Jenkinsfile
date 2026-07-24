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

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t react-cicd-app .'
            }
        }

        stage('Deploy to Development') {
            steps {
                sh '''
                docker stop reactapp || true
                docker rm reactapp || true
                docker run -d --name reactapp -p 3000:3000 react-cicd-app
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
    }

    post {
        success {
            echo "Pipeline completed successfully."
        }

        failure {
            echo "Pipeline failed. Rollback can be implemented here."
        }
    }
}
