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

                    echo "i think am tired now sir, if there is nothing else i thenk i will pull some sleep too :)"
                }
            }
        }
    }
    post {
        failure {
            emailext (
                subject: "Build Failed 😔: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                    <p>The build for <b>${env.JOB_NAME}</b> has failed.</p>
                    <p>Check the console output at: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                """,
                recipientProviders: [[$class: 'CulpritRecipientProvider'], [$class: 'RequesterRecipientProvider']],
                mimeType: 'text/html'
            )
        }
        success {
            emailext (
                subject: "I think the build was successful 😄: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                    <p>The build test for <b>${env.JOB_NAME}</b> has run successfully.</p>
                    <p>The initiator was <b>${env.CHANGE_AUTHOR}</b>, email: <b>${env.CHANGE_AUTHOR_EMAIL}</b></p>
                    <p><b>I think I'm going to sleep now. Is there anything else you need?</b></p>
                """,
                recipientProviders: [$class: 'CulpritRecipientProvider'],
                mimeType: 'text/html'
            )
        }
    }
}
