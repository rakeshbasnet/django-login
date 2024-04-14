pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'python:3' // Base Docker image with Python 3.8
        PROJECT_DIR = '/app' // Path to your Django project directory
    }

    stages {
        stage('Check Code Style with Flake8') {
            steps {
                script {
                    // Pull the Docker image
                    docker.image("${DOCKER_IMAGE}").pull()

                    // Run Flake8 inside the Docker container
                    def flake8Result = docker.image("${DOCKER_IMAGE}").inside("-v $PWD:${PROJECT_DIR}") {
                        sh "pip install flake8"
                        sh "flake8 ${PROJECT_DIR}"
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
    }
}
