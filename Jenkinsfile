pipeline {
    agent any

    parameters {
        string(name: 'BUILD_VERSION', defaultValue: '1.0.0', description: 'Version number for the build')
    }

    environment {
        ARTIFACTORY_SERVER = 'your-artifactory-server-id'  // Jenkins global Artifactory Server ID
        ARTIFACTORY_REPO = 'libs-release-local'             // Target repo in Artifactory
        MAVEN_TOOL = 'M3'                                   // Maven tool name configured in Jenkins (Manage Jenkins > Global Tool Config)
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                script {
                    def mvnHome = tool "${env.MAVEN_TOOL}"
                    sh "${mvnHome}/bin/mvn clean package -Drevision=${params.BUILD_VERSION}"
                }
            }
        }

        stage('Publish to Artifactory') {
            steps {
                script {
                    def server = Artifactory.server("${env.ARTIFACTORY_SERVER}")
                    def uploadSpec = """{
                      "files": [{
                        "pattern": "target/*.jar",
                        "target": "${ARTIFACTORY_REPO}/simple-java-maven-app/${params.BUILD_VERSION}/"
                      }]
                    }"""
                    server.upload spec: uploadSpec
                }
            }
        }
    }

    post {
        success {
            echo "Build and upload successful!"
        }
        failure {
            echo "Build failed."
        }
    }
}
