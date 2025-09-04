pipeline {
    agent any

    environment {
        APP_NAME = 'flask-demo'
        IMAGE = "${env.DOCKERHUB_USER ?: 'local'}/${APP_NAME}"
        TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                sh "docker build -t ${IMAGE}:${TAG} ."
                sh "docker tag ${IMAGE}:${TAG} ${IMAGE}:latest"
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker rm -f demo || true'
                sh "docker run -d --name demo -p 5000:5000 ${IMAGE}:latest"
            }
        }

        stage('Smoke Test (optional)') {
            steps {
                sh 'sleep 5 || true'
                sh 'curl -fsS http://localhost:5000/ | tee smoke.json || true'
            }
        }
    }
}

