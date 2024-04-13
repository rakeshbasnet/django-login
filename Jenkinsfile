pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'python:3' // Base Docker image with Python 3.8
        PROJECT_NAME = 'django_login'
        DOCKER_REPO = 'rakeshbasnet/django-login' // Docker Hub repository name
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Pull the Docker image
                    docker.image("${DOCKER_IMAGE}").pull()

                    // Run Flake8 to lint the code
                    def flake8Result = docker.image("${DOCKER_IMAGE}").inside('-v $PWD:/app -v $HOME/.cache:/root/.cache') {
                        sh 'pip install --user flake8'
                        sh 'flake8'
                    }
                    
                    // Check if Flake8 passed successfully
                    if (flake8Result == 0) {
                        echo 'Flake8 passed successfully'
                    } else {
                        error 'Flake8 found code style violations'
                    }

                    // Install Django
                    def flake8Result = docker.image("${DOCKER_IMAGE}").inside('-v $PWD:/app -v $HOME/.cache:/root/.cache') {
                        sh 'pip install --user django'
                    }

                    // Run Django project
                    docker.image("${DOCKER_IMAGE}").inside('-v $PWD:/app') {
                        sh 'python manage.py migrate'
                        sh 'python manage.py runserver'
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
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

    post {
        always {
            // Clean up workspace after build
            cleanWs()
        }
    }
}