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

        stages {
        stage('Build') {
            steps {
                dir('jenkins-python-demo-app') {
                    sh '''
                        python3 -m venv venv
                        . venv/bin/activate
                        pip install --upgrade pip
                        pip install -r requirements.txt
                    '''
                }
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

