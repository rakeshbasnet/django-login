pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'pip install -r requirements.txt'
                sh 'flake8 .'
            }
        }
        stage('Test') {
            steps {
                sh 'python manage.py test'
            }
        }
    }
}