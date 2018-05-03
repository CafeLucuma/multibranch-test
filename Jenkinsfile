pipeline {
    agent any
    tools {
        maven 'maven'
        jdk 'jdk'
    }
    stages {
        stage ('Common'){
            steps{
                echo "printing env"
                sh 'printenv'
            }
        }
        stage('Deliver for development') {
            when {
                branch 'development'
            }
            steps {
                echo "running development"
                sh 'mvn clean install'
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

