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
        stage('Run Ansible Playbook') {
            steps {
                script {
                    // Define details of the remote Ansible server
                    def remoteServer = '172.31.19.31'
                    def username = 'root'
                    def privateKey = 'ansible-key' // Assuming SSH key authentication
                    def playbookPath = 'opt/ansible/deploy.yml' // Path on the remote server

                    // SSH options for secure connection
                    def sshOptions = "-o StrictHostKeyChecking=no -i ${privateKey}"

                    // Run ansible-playbook using SSH
                    sh "ssh ${sshOptions} ${username}@${remoteServer} ansible-playbook ${playbookPath}"
                }
            }
        }
    }
}