pipeline {
    agent any  // Gunakan agent default
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                script {
                    docker.image('node:lts-buster-slim').inside('-p 3000:3000') {
                        sh 'npm install'
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    docker.image('node:lts-buster-slim').inside('-p 3000:3000') {
                        sh './jenkins/scripts/test.sh'
                    }
                }
            }
        }
        stage('Deliver') {
            steps {
                script {
                    docker.image('node:lts-buster-slim').inside('-p 3000:3000') {
                        sh './jenkins/scripts/deliver.sh'
                        input message: 'Finished using the website? (Click "Proceed" to continue)'
                        sh './jenkins/scripts/kill.sh'
                    }
                }
            }
        }
    }
}
