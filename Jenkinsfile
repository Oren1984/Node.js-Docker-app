pipeline {
    agent any

    environment {
        IMAGE_NAME = 'yourdockerhubuser/nodejs-app'
        IMAGE_VERSION = '1.0.0'
        DOCKER_CREDS = credentials('dockerhub')
    }

    stages {
        stage('Code Checkout') {
            steps {
                git "https://github.com/Oren1984/Node.js-Docker-app.git"
            }
        }

        stage('Docker Build + Tag') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_VERSION .'
            }
        }

        stage('Docker Push') {
            steps {
                sh 'echo $DOCKER_CREDS_PSW | docker login -u $DOCKER_CREDS_USR --password-stdin'
                sh 'docker push $IMAGE_NAME:$IMAGE_VERSION'
            }
        }

        stage('Docker Run') {
            steps {
                sh 'docker run -d -p 3000:3000 $IMAGE_NAME:$IMAGE_VERSION'
            }
        }
    }
}
