pipeline {
    agent any

    // Add Maven tool defined in Jenkins Global Tool Configuration
    tools {
        maven 'M3'  // This should match the name you configured in Global Tool Configuration
    }

    // Define input parameter for deployment environment
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

        stage('Configure JFrog CLI') {
            steps {
                script {
                    echo "Configuring JFrog CLI..."
                    // Configure JFrog CLI with Artifactory URL and credentials
                    sh "jf rt config --url https://mycompany.jfrog.io/artifactory --user ${ARTIFACTORY_USER} --apikey ${ARTIFACTORY_API_KEY}"
                }
            }
        }

        stage('Upload to Artifactory') {
            steps {
                script {
                    echo "Uploading artifacts to Artifactory..."
                    // Upload the artifacts to JFrog Artifactory
                    sh "jf rt u 'target/*.jar' 'libs-release-local/com/mycompany/myapp/${params.DEPLOY_ENV}/'"
                }
            }
        }

        stage('Post-Build Action') {
            steps {
                script {
                    echo "Post-build step for environment: ${params.DEPLOY_ENV}"

                    if (params.DEPLOY_ENV == 'prod') {
                        echo "You selected production. Deploy to PROD cluster."
                        // TODO: Add real production deployment logic here
                    } else if (params.DEPLOY_ENV == 'test') {
                        echo "You selected test. Deploy to TEST cluster."
                        // TODO: Add real test deployment logic here
                    } else {
                        echo "You selected dev. Deploy to DEV cluster."
                        // TODO: Add real dev deployment logic here
                    }
                }
            }
        }
    }

    // Post build actions
    post {
        success {
            echo "Build and deployment completed successfully!"
        }
        failure {
            echo "Build or deployment failed!"
        }
    }
}

