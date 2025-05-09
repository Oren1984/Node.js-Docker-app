pipeline {
    agent any

    environment {
        IMAGE_NAME = 'yourdockerhubuser/nodejs-app'
        IMAGE_VERSION = '1.0.0'
        DOCKER_CREDS = credentials('dockerhub') // ודא ש-Credentials ID הזה קיים
    }

    stages {
        stage('Code Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Oren1984/Node.js-Docker-app.git'
            }
        }

        stage('Docker Build + Tag') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_VERSION .'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push $IMAGE_NAME:$IMAGE_VERSION
                    '''
                }
            }
        }

        stage('Docker Run') {
            steps {
                sh 'docker run -d -p 3000:3000 $IMAGE_NAME:$IMAGE_VERSION'
            }
        }
    }
}
