pipeline {
  agent {
    docker { image 'python3' }
  }
  stages {
    stage('Test') {
      steps {
        sh 'python3 --version'
      }
    }
  }
}