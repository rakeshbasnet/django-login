pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'python:3.8' // Base Docker image with Python 3.8
        PROJECT_DIR = '/app' // Path to your Django project directory
        DOCKER_REPO = 'rakeshbasnet/django-login' // Docker Hub repository name
    }

    stages {
        stage('Check Code Style with Flake8') {
            steps {
                script {
                    // Pull the Docker image
                    docker.image("${DOCKER_IMAGE}").pull()

                    // Run Flake8 inside the Docker container
                    def flake8Result = docker.image("${DOCKER_IMAGE}").run('-v $PWD:/app') {
                      sh 'python -m venv venv'
                      sh 'source venv/bin/activate'
                      sh 'pip install flake8'
                      sh 'flake8'
                    }


                    // Check Flake8 result
                    if (flake8Result == 0) {
                        echo 'Flake8 check passed'
                    } else {
                        error 'Flake8 found code style violations'
                    }
                }
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Build Docker image
                    docker.build("${DOCKER_REPO}:${BUILD_NUMBER}", '.')

                    // Push Docker image to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
                        docker.image("${DOCKER_REPO}:${BUILD_NUMBER}").push('latest')
                    }
                }
            }
        }
    }
}