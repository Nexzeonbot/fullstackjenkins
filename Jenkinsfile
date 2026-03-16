pipeline {
    agent any

    environment {
        FRONTEND_DIR = "frontend"
        BACKEND_DIR  = "backend"
        EC2_HOST     = "ec2-user@ec2-13-54-226-145.ap-southeast-2.compute.amazonaws.com"
        KEY_PATH     = "fullstackjenkins.pem"
        S3_BUCKET    = "s3://fullstackjen1"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Nexzeonbot/fullstackjenkins.git'
            }
        }

        stage('Install Backend Dependencies') {
            steps {
                dir("${BACKEND_DIR}") {
                    sh 'npm install'
                }
            }
        }

        stage('Install Frontend Dependencies') {
            steps {
                dir("${FRONTEND_DIR}") {
                    sh 'npm install'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir("${FRONTEND_DIR}") {
                    sh 'npm run build'
                }
            }
        }

        stage('Deploy Backend to EC2') {
            steps {
                sh '''
                echo "Deploying Backend to EC2"

                ssh -i ${KEY_PATH} ${EC2_HOST} "rm -rf backend"

                scp -i ${KEY_PATH} -r backend ${EC2_HOST}:~

                ssh -i ${KEY_PATH} ${EC2_HOST} "
                    cd backend &&
                    npm install --production &&
                    sudo systemctl restart backend
                "
                '''
            }
        }

        stage('Deploy Frontend to S3') {
            steps {
                sh '''
                echo "Uploading Frontend to S3"

                if [ -d "frontend/build" ]; then
                    aws s3 sync frontend/build ${S3_BUCKET} --delete
                elif [ -d "frontend/dist" ]; then
                    aws s3 sync frontend/dist ${S3_BUCKET} --delete
                else
                    echo "Build folder not found!"
                    exit 1
                fi
                '''
            }
        }

    }

    post {
        success {
            echo "Deployment Successful"
        }
        failure {
            echo "Deployment Failed"
        }
    }
}
