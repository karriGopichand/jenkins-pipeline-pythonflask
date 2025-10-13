pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-credentials' // Jenkins credentials ID
        IMAGE_NAME = 'yourdockerhubusername/python-cicd-demo'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/karriGopichand/jenkins-pipeline-pythonflask.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "$DOCKERHUB_CREDENTIALS", passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker stop python-cicd-demo || true'
                sh 'docker rm python-cicd-demo || true'
                sh 'docker run -d -p 5000:5000 --name python-cicd-demo $IMAGE_NAME'
            }
        }
    }
}
