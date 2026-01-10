pipeline {
    agent any

    tools {
        maven 'M3'
    }

    environment {
        SLACK_CHANNEL = '#project-devops'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/AnassChahid/Projet-DevOps-AnassChahid'
            }
        }

        stage('Build & Test') {
            steps {
                dir('application') {
                    bat 'mvn clean test package'
                }
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'application/target/application-0.0.1-SNAPSHOT.jar'
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploying...'
            }
        }
    }

    post {
        success {
            withCredentials([string(credentialsId: 'slack-webhook', variable: 'SLACK_WEBHOOK')]) {
                slackSend(
                    webhookUrl: env.SLACK_WEBHOOK,
                    message: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
                )
            }
        }
        failure {
            withCredentials([string(credentialsId: 'slack-webhook', variable: 'SLACK_WEBHOOK')]) {
                slackSend(
                    webhookUrl: env.SLACK_WEBHOOK,
                    message: "❌ FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
                )
            }
        }
    }
}
