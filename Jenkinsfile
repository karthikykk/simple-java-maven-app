pipeline {
    agent any

    tools {
        maven 'Maven 3.x' // Ensure this matches your Jenkins Maven configuration name
    }

    environment {
        S3_BUCKET = 'jenkins-artifacts-123456789'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                git 'https://github.com/karthikykk/simple-java-maven-app.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Running Maven build...'
                sh 'mvn clean install'
            }
        }

        stage('Upload to S3') {
            steps {
                script {
                    def timestamp = sh(script: "date +%Y%m%d%H%M%S", returnStdout: true).trim()
                    def artifactPath = "target/my-app-1.0-SNAPSHOT.jar"

                    echo "Uploading build artifact to S3 as build_${timestamp}.jar..."
                    sh "aws s3 cp ${artifactPath} s3://${S3_BUCKET}/builds/build_${timestamp}.jar"
                }
            }
        }
    }

    post {
        failure {
            echo 'Build or upload failed.'
        }
        success {
            echo 'Build and upload completed successfully.'
        }
    }
}
