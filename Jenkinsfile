pipeline {
    agent any

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/Nexzeonbot/fullstackjenkins.git'
            }
        }

        stage('Deploy Backend to EC2') {
            steps {
                sh '''
                ssh -o StrictHostKeyChecking=no ec2-user@13.54.226.145 << EOF
                cd /home/ec2-user/fullstackjenkins
                git pull origin main
                npm install
                sudo pkill -f node
                nohup node app.js > app.log 2>&1 &
                EOF
                '''
            }
        }

    }
}
