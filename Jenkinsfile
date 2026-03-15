pipeline {
    agent any

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/Nexzeonbot/fullstackjenkins.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Start Application') {
            steps {
                sh 'nohup node app.js &'
            }
        }

    }
}
