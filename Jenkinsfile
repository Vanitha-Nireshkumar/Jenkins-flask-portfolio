pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('DockerHub')
        IMAGE_NAME = 'vanithanireshkumar/flask-portfolio'
    }

    stages {
        stage('test'){
            steps{
                echo"this is testing"
            }
        }
        stage('Checkout Code') {
            steps {
                git 'https://github.com/theshubhamgour/flask-portfolio.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:Latest .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                    sh 'docker push $IMAGE_NAME:Latest'
                }
            }
        }

        stage('Deploy to Stage') {
            steps {
                sh '''docker rm -f $IMAGE_NAME:Latest || true
                docker run -d -p 5000:5000 $IMAGE_NAME:Latest'''
            }
        }
    }

    post {
        success { echo '✅ Build, Test, and Deploy completed successfully!' }
        failure { echo '❌ Pipeline failed. Check logs.' }
    }
}
