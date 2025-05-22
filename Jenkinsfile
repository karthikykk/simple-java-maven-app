pipeline {
    agent any

    tools {
        maven 'M3' // This must match the name set in Jenkins Global Tool Configuration
    }

    environment {
        AWS_DEFAULT_REGION = 'us-east-1' // Replace with your region
        BUCKET_NAME = 'jenkins-artifacts-123456789'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out the code from GitHub...'
                git 'https://github.com/karthikykk/simple-java-maven-app.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Running Maven build...'
                sh 'mvn clean install'
            }
        }

        stage('List Files') {
            steps {
                echo 'Listing files in target directory...'
                sh 'ls -lh target'
            }
        }

        stage('Upload to S3') {
            steps {
                script {
                    def timestamp = sh(script: "date +%Y%m%d%H%M%S", returnStdout: true).trim()
                    def filePath = "target/my-app-1.0-SNAPSHOT.jar"
                    def s3Key = "builds/build_${timestamp}.jar"

                    echo "Uploading ${filePath} to s3://${env.BUCKET_NAME}/${s3Key} ..."
                    sh "aws s3 cp ${filePath} s3://${env.BUCKET_NAME}/${s3Key}"
                }
            }
        }
    }

    post {
        success {
            echo 'Build and upload succeeded!'
        }
        failure {
            echo 'Build or upload failed.'
        }
    }
}
