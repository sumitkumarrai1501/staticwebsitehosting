pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        S3_BUCKET = 'besant-demo-website'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/yashaspancham/frontend_s3_web_hosting.git',
                    credentialsId: 'AWS-creds'
            }
        }

        stage('Build React') {
            steps {
                sh 'chmod +x build.sh'
                sh './build.sh'
            }
        }

        stage('Deploy to S3 & Invalidate CloudFront') {
            steps {
                withAWS(credentials: 'AWS-creds', region: 'ap-south-1') {
                    sh 'aws s3 sync build/ s3://$S3_BUCKET --delete'
                    sh 'aws cloudfront create-invalidation --distribution-id E1W5YWBNW5A7S7 --paths "/*"'
                }
            }
        }
    }
}
