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

                    if (params.DEPLOY_ENV == 'prod') {
                        echo "Production environment selected. Add PROD deployment logic here."
                    } else {
                        echo "Non-production environment. Add DEV/TEST deployment logic here."
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build or deployment failed!'
        }
    }
}

