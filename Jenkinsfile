pipeline {
    agent any

    tools {
        maven 'M3' // This must match the Maven tool name configured in Jenkins global tools
    }

    parameters {
        string(name: 'DEPLOY_ENV', defaultValue: 'dev', description: 'Deployment environment (e.g., dev, test, prod)')
    }

    environment {
        MAVEN_OPTS = "-Xms256m -Xmx512m"
        S3_BUCKET_NAME = 'jenkins-artifacts-123456789'  // Replace with your S3 bucket name
        S3_FOLDER = 'builds'  // Optional: Specify folder inside the S3 bucket
    }

    stages {
        stage('Initialize') {
            steps {
                echo "Pipeline started for environment: ${params.DEPLOY_ENV}"
            }
        }

        stage('Checkout') {
            steps {
                echo "Checking out the code from GitHub..."
                git branch: 'master',
                    url: 'https://github.com/karthikykk/simple-java-maven-app.git'
            }
        }

        stage('Build') {
            steps {
                echo "Running Maven build..."
                sh 'mvn clean install'
            }
        }

        stage('Post-Build Action') {
            steps {
                script {
                    echo "Post-build step for environment: ${params.DEPLOY_ENV}"

                    // Get the timestamp to create unique file names
                    def timestamp = sh(script: "date +%Y%m%d%H%M%S", returnStdout: true).trim()

                    // Path to the build artifact (target folder is usually where Maven places the .jar file)
                    def buildDir = 'target'  // Adjust this if your build artifact is in a different location
                    def artifactName = 'simple-java-maven-app.jar'  // Replace with the actual artifact name from your build

                    // Check if environment is production or non-production
                    if (params.DEPLOY_ENV == 'prod') {
                        echo "Production environment selected. Add PROD deployment logic here."
                    } else {
                        echo "Non-production environment. Add DEV/TEST deployment logic here."

                        // Upload artifact to S3
                        echo "Uploading build artifact to S3..."
                        sh """
                            aws s3 cp ${buildDir}/${artifactName} s3://${S3_BUCKET_NAME}/${S3_FOLDER}/build_${timestamp}.jar
                        """
                    }
                }
            }
        }
    }
}

