pipeline {
    agent any 

    environment {
        DOCKER_IMAGE = 'your-dockerhub-karrigopichand/your-image-name:latest'
        DOCKER_CREDENTIALS = 'dockerhub-credentials'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

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
                dir('jenkins-python-demo-app') {
                    script {
                        docker.build(DOCKER_IMAGE)
                    }
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_CREDENTIALS) {
                        docker.image(DOCKER_IMAGE).push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploy your application here.'
                // Add your deployment commands/scripts here
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
