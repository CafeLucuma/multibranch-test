pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('Deliver for development') {
            when {
                branch 'development'
            }
            steps {
                sshagent (credentials: ['ssh-local']) {
                    sh 'ssh -o StrictHostKeyChecking=no admin1@192.168.0.158 uname -a'
                    sh 'whoami'
                          
                }
                echo "Running for development"
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
            }
        }
        stage('Deploy for production') {
            when {
                branch 'production'
            }
            steps {
                echo "Running for production"
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
            }
        }
    }
}

