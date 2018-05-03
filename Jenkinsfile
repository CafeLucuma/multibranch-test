pipeline {
    agent any
    environment { 
        git_creds = credentials('github-credentials')
    }
    tools {
        maven 'maven'
        jdk 'jdk'
    }
    stages {
        stage ('Common') {
            steps{
                echo "printing env"
                sh 'printenv'
                sh 'mvn clean install'
            }
        }

        stage('push') {
            steps{
                sh("git tag -a ${BUILD_NUMBER} -m 'Jenkins'")
                sh("git push https://$env.git_creds_USR:$env.git_creds_PSW@github.com/CafeLucuma/multibranch-test.git HEAD:master --tags")
            } 
            post{
                success {
                    options {
                        timeout(time: 1, unit: 'MINUTES') 
                    }
                    echo "Push exitoso a master, actualizando jars..."
                    sh 'mvn deploy'
                }
                failure {
                    echo "Error al hacer push"
                }
            }
            
        }
    }
}

