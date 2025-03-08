pipeline {
    agent {
        docker {
            image 'node:lts-buster-slim'
            args '-p 3000:3000'
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Manual Approval') { // Menambahkan stage persetujuan manual sebelum Deploy
            steps {
                input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed'
            }
        }
        stage('Deploy') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                echo 'Aplikasi sedang berjalan. Menunggu selama 1 menit sebelum otomatis dihentikan...'
                sh 'sleep 60' // Menjeda eksekusi pipeline selama 1 menit
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}