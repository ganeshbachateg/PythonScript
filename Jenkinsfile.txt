pipeline {
    agent any

    stages {
        stage('List S3 Buckets') {
            steps {
                sh '''
                    #!/bin/bash
                    pip install boto3

                    python << EOF
import boto3

s3 = boto3.client('s3')  # boto3 automatically uses the instance's IAM role

try:
    response = s3.list_buckets()
    for bucket in response['Buckets']:
        print(bucket['Name'])
except Exception as e:
    print(f"Error: {e}")
EOF
                '''
            }
        }
    }
}