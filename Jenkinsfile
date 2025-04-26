pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'prathamchouhan'
        IMAGE_NAME = 'flask-app-pratham'
        // This credential ID created in Jenkins Credentials
        DOCKER_CREDENTIALS_ID = 'dockerhub-credentials-pratham'
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Cloning repository...'
                git branch: 'main', url:'https://github.com/chouhanpratham/flask-app.git'
            }
        }

        stage('Docker Login') {
            steps {
                echo 'Logging into Docker Hub...'
                withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:latest ."
            }
        }

        stage('Push Docker Image') {
            steps {
                echo 'Pushing image to Docker Hub...'
                sh "docker push ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:latest"
            }
        }
    }

    post {
        success {
            echo 'Docker image built and pushed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
