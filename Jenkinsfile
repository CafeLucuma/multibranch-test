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
        stage('Sonar analysis') {
            steps {
                sh 'printenv'
                sh 'ls'
                sh 'mv compile'
                script {
                    echo "Inicializando sonar"
                    def scanner = tool 'SonarScanner';
                    withSonarQubeEnv('SonarQube_Akzio') {
                        echo "Sonar scanner path: " + scanner
                        sh "${scanner}/bin/sonar-scanner -X"
                    }
                }
                echo "Analisis terminado"
            }
        }

        stage("SonarQube Quality Gate") {
            options{
                timeout(time: 1, unit: 'HOURS')
            }
            steps{
                echo "Esperando resultado de quality gate..."
                script {
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    }
                    echo "Estado de ĺa quality gate: ${qg.status}"
                }
            }
            post {
                success {
                    echo "Analisis de sonarQube ok, pasando a unit testing"
                }
            }
        }

        stage ('Unit testing') {
            steps{
                sh 'mvn clean install'
            }
            post {
                failure {
                    echo "Fallaron los tests"
                }
            }
        }

        stage('push') {
            steps{
                sh("git tag -a ${BUILD_NUMBER} -m 'Jenkins'")
                sh("git push https://$env.git_creds_USR:$env.git_creds_PSW@github.com/CafeLucuma/multibranch-test.git HEAD:master --tags")
            } 
            post {
                success {
                    echo "Push exitoso a master, actualizando jars..."
                    timeout(time: 20, unit: 'SECONDS'){
                        sh 'mvn deploy'
                    }
                }
                failure {
                    echo "Error al hacer push"
                }
            }
            
        }
    }
}

