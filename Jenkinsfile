pipeline {
    agent any
    environment {
        RENDER_DEPLOY_URL = 'https://api.render.com/deploy/srv-crmk3vo8fa8c73ak0q5g'
        RENDER_API_KEY = credentials('ccfca321-a4e8-4c64-9ff5-1848aea5b33c') 
    }
    tools {
        nodejs 'nodejs'
    }
    stages {
        stage('Cloning Repo') {
            steps {
                git 'https://github.com/rashidjosphat/gallery_ip1.git'
            }
        }
        stage('Installing Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Testing the Application') {
            steps {
                sh 'npm test'
            }
        }
        stage('Activation of Deployment') {
            steps {
                script {
                    def response = sh(script: """
                        curl -X POST '${RENDER_DEPLOY_URL}?key=${RENDER_API_KEY}' \
                        -H 'Content-Type: application/json' \
                        -d '{}' 
                    """, returnStdout: true).trim()

                    echo "Deployment triggered successfully via Render. If nothing else, it's time for some rest. 😊"
                }
            }
        }
    }
    post {
        failure {
            emailext (
                to: 'jamesrashid226@gmail.com',
                subject: "Build Failed 😔: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                    The build failed for ${env.JOB_NAME} #${env.BUILD_NUMBER}.
                """
            )
            // Slack notification on failure
            slackSend(channel: '#social', message: "Build Failed 😔: ${env.JOB_NAME} #${env.BUILD_NUMBER}. Render Deployment URL: ${RENDER_DEPLOY_URL}")
        }
        success {
            emailext (
                to: 'jamesrashid226@gmail.com',
                subject: "Build Succeeded 🎉: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                    The build was successful for ${env.JOB_NAME} #${env.BUILD_NUMBER}.
                    
                    Build URL: ${env.BUILD_URL}
                """
            )
            // Slack notification on success
            slackSend(channel: '#social', message: "Build succeeded 🎉: ${env.JOB_NAME} #${env.BUILD_NUMBER}. Render Deployment URL: ${RENDER_DEPLOY_URL}")
        }
    }
}
