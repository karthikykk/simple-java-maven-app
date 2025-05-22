pipeline {
    agent any

    // Add Maven tool defined in Jenkins Global Tool Configuration
    tools {
         maven 'M3'  // This should match the name you configured in Global Tool Configuration
    }

    // Define input parameter
    parameters {
        string(name: 'DEPLOY_ENV', defaultValue: 'dev', description: 'Deployment environment (e.g., dev, test, prod)')
    }

    environment {
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
                        // TODO: Add real production deployment logic
                    } else if (params.DEPLOY_ENV == 'test') {
                        echo "Test environment selected. Deploying to TEST environment..."
                        // TODO: Add real test deployment logic
                    } else {
                        echo "Non-prod/test environment. Deploying to DEV environment..."
                        // TODO: Add real dev deployment logic
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully for ${params.DEPLOY_ENV}"
        }
        failure {
            echo "Build failed for ${params.DEPLOY_ENV}"
        }
    }
}

