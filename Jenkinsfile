pipeline {
    agent any

    environment {
        AWS_CREDENTIALS = credentials('aws-credentials-id')
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo-owner/your-repo-name.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Application') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy to S3') {
            steps {
                script {
                    sh '''
                    # Sync build output to S3 bucket
                    aws s3 sync build/ s3://your-s3-bucket-name/
                    '''
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    sh '''
                    # Deploy to EC2 instance
                    ssh -i /path/to/your-key.pem ec2-user@your-ec2-instance << EOF
                    sudo aws s3 sync s3://your-s3-bucket-name/ /var/www/html/
                    sudo systemctl restart nginx
                    EOF
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}

