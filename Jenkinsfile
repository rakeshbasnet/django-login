pipeline {
    agent any
    
    environment {
        DOCKER_REPO = 'rakeshbasnet/django-login' // Docker Hub repository name
    }

    stages {
        
        stage('Build Docker'){
            steps{
                script{
                  // Build Docker image
                    sh '''
                    echo 'Buid Docker Image'
                    docker build -t ${DOCKER_REPO}:${BUILD_NUMBER} .
                    echo 'Docker build successfully.'
                    '''
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {

                    // Push Docker image to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
                        docker.image("${DOCKER_REPO}:${BUILD_NUMBER}").push('latest')
                    }
                }
            }
        }
        stage('Execute Ansible Playbook') {
            steps {
                script {

                  ansiblePlaybook credentialsId: 'ansible-key', disableHostKeyChecking: true, installation: 'Ansible', inventory: '/etc/ansible/hosts', playbook: '/opt/ansible/deploy.yml', vaultTmpPath: ''
                }
            }
        }
    }
}