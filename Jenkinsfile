pipeline {
    agent any

    // Define input parameter
    parameters {
        string(name: 'DEPLOY_ENV', defaultValue: 'dev', description: 'Deployment environment (e.g., dev, test, prod)')
    }

    environment {
        // Optional: define environment variables if needed
        MAVEN_OPTS = "-Xms256m -Xmx512m"
    }

    stages {

        stage('Initialize') {
            steps {
                echo "Pipeline started for environment: ${params.DEPLOY_ENV}"
            }
        }

        stage('Checkout') {
            steps {
                // Clone source code from Git
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
                    if (params.DEPLOY_ENV == 'prod') {
                        echo "You selected production. Deploy to PROD cluster."
                        // Place production deploy logic here
                    } else if (params.DEPLOY_ENV == 'test') {
                        echo "Test environment selected. Deploying to TEST environment..."
                        // Place test deploy logic here
                    } else {
                        echo "Default or dev environment. Deploying to DEV..."
                        // Place dev deploy logic here
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Build and steps completed successfully for ${params.DEPLOY_ENV}"
        }
        failure {
            echo "Build failed for ${params.DEPLOY_ENV}"
        }
    }
}

