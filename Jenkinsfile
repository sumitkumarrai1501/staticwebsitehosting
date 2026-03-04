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
                    url: 'https://github.com/sumitkumarrai1501/staticwebsitehosting.git',
                    credentialsId: 'sumitkumarrai'
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
                withAWS(credentials: 'sumitkumarrai', region: 'us-east-1') {
                    sh 'aws s3 sync build/ s3://$S3_BUCKET --delete'
                    sh 'aws cloudfront create-invalidation --distribution-id E1AZ06VD3QJ51K --paths "/*"'
                }
            }
        }
    }
}
