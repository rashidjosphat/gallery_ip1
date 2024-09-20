pipeline {
    agent any
    environment {
        RENDER_DEPLOY_URL = 'https://api.render.com/deploy/srv-crm2jibv2p9s73e7fbcg'
        RENDER_API_KEY = credentials('render_api_key') 
    }
    tools {
        nodejs 'nodejs'
    }
    stages {
        stage('Cloning Repo') {
            steps {
                git 'https://github.com/jamesrashid226/gallery.git'
            }
        }
        stage('Installing Dependencies') {
            steps {
                sh 'npm install'
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
}
