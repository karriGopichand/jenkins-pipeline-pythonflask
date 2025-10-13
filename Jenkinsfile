pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-python-app"
        REGISTRY = ""  // set your docker registry here, or leave empty if local
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Installing dependencies and running tests (if any)'
                sh 'pip install -r requirements.txt'
                // Add tests here if you want
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Docker Push') {
            when {
                expression { env.REGISTRY != "" }
            }
            steps {
                script {
                    docker.withRegistry("https://${env.REGISTRY}", 'docker-credentials-id') {
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Docker container'
                sh "docker rm -f ${IMAGE_NAME} || true"
                sh "docker run -d --name ${IMAGE_NAME} -p 5000:5000 ${IMAGE_NAME}:latest"
            }
        }
    }
}

