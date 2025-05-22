pipeline {
    agent any

    environment {
        // Define environment variables here
        S3_BUCKET = "jenkins-artifacts-123456789"
        ARTIFACT_NAME = "my-app-1.0-SNAPSHOT.jar"  // Update this to match your actual artifact name
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out the code from GitHub..."
                git 'https://github.com/karthikykk/simple-java-maven-app.git'
            }
        }

        stage('Build') {
            steps {
                echo "Running Maven build..."
                script {
                    // Run Maven build
                    sh 'mvn clean install'
                }
            }
        }

        stage('List Files') {
            steps {
                echo "Listing files in target/ directory..."
                sh 'ls -al target/'
            }
        }

        stage('Upload to S3') {
            steps {
                script {
                    // Check if the artifact exists before attempting to upload
                    def artifactPath = "target/${ARTIFACT_NAME}"
                    if (fileExists(artifactPath)) {
                        echo "Artifact exists, uploading to S3..."
                        def timestamp = sh(script: 'date +%Y%m%d%H%M%S', returnStdout: true).trim()
                        sh """
                            aws s3 cp ${artifactPath} s3://${S3_BUCKET}/builds/build_${timestamp}.jar
                        """
                    } else {
                        error "Artifact ${artifactPath} does not exist, aborting upload."
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Build and upload to S3 completed successfully.'
        }
        failure {
            echo 'Build or upload failed.'
        }
    }
}
