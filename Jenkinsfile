pipeline {
agent any

```
stages {

    stage('Clone Repo') {
        steps {
            git 'https://github.com/Nexzeonbot/fullstackjenkins.git'
        }
    }

    stage('Deploy Frontend to S3') {
        steps {
            sh '''
            aws s3 sync frontend/ s3://fullstackjen1 --delete
            '''
        }
    }

    stage('Restart Backend') {
        steps {
            sh '''
            ssh ec2-user@13.54.226.145 "pkill -f app.py"
            ssh ec2-user@13.54.226.145 "nohup python3 app.py &"
            '''
        }
    }

}
```

}

