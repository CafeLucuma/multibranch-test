pipeline {
    agent any
    environment { 
        git-creds = credentials('github-credentials')
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
            }
        }
        stage('push') {
            sh("git tag -a ${BUILD_NUMBER} -m 'Jenkins'")
            sh("git push https://$env.git-creds_USR:env.git-creds_PSW@github.com/CafeLucuma/multibranch-test.git HEAD:master --tags")
        }
    }
}

