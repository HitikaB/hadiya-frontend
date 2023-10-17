pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'ap-south-1'
        S3_BUCKET_NAME = 'tf-s3-hitika'
        CLOUDFRONT_DISTRIBUTION_ID = 'EMOEAYI0HZKBB'
    }
    
    tools {
        nodejs 'hitika'
    }
    
    stages {
        stage("Git Checkout") {
            steps {
                git credentialsId: 'hitika', url: 'https://github.com/HitikaB/hadiya-backend.git', branch: "main"
                sh 'ls -la'
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run'
            }
        }
        
        stage('Deploy Artifacts') {
            steps {
                script {
                    // Assuming your build outputs are in a 'dist' directory
                    sh "aws s3 cp dist/* s3://${S3_BUCKET_NAME}/ --recursive"
                }
            }
        }
        
        stage('Invalidate CloudFront Cache') {
            steps {
                script {
                    sh "aws cloudfront create-invalidation --distribution-id ${CLOUDFRONT_DISTRIBUTION_ID} --paths '/*'"
                }
            }
        }
    }
}
