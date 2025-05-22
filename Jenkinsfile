pipeline {
    agent any

    tools {
        maven 'Maven 3.x' // Match this name with what you configured in Jenkins -> Global Tool Configuration
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
                    def timestamp = sh(scr
