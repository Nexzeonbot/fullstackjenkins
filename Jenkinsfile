pipeline {
agent any

```
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
            cd /home/ec2-user
            pkill -f app.py
            nohup python3 app.py &
            EOF
            '''
        }
    }

    stage('Deploy Frontend to S3') {
        steps {
            sh '''
            aws s3 sync frontend/ s3://fullstackjen1
            '''
        }
    }

}
```

}

