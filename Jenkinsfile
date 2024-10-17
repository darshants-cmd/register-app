pipeline {
    agent any
    tools {
        jdk 'java17'
        maven 'Maven3'
    }

    environment {
        IMAGE_NAME = 'darshantsd/register-app1'
        IMAGE_TAG = 'latest'
        JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from SCM') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/darshants-cmd/register-app'
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test Application') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') { // Assuming 'SonarQube' is the server name configured in Jenkins
                        sh "mvn sonar:sonar"
                    }
                }
            }
        }
    }
}
