pipeline {
    agent any

    stages {
        stage('Run Commands on Remote Server') {
            steps {
                script {
                    // Define details of the remote server
                    def remoteServer = '172.31.19.31'
                    def username = 'root'
                    def privateKey = '/etc/ssh/ansible-id-rsa' // Assuming SSH key authentication
                    def commands = """
                        # List of commands to execute on the remote server
                        ls -l /var/www
                        cat /etc/os-release
                    """

                    // SSH options for secure connection
                    def sshOptions = "-o StrictHostKeyChecking=no -i ${privateKey}"

                    // Run commands using SSH
                    sh """
                        ssh ${username}@${remoteServer} << 'EOF'
                        ${commands}
                        EOF
                    """
                }
            }
        }
    }
}