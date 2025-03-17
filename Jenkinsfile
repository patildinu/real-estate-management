pipeline {
    agent any

    environment {
        NODE_VERSION = '16'  // Ensure compatibility with your project
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/patildinu/real-estate-management'
            }
        }

        stage('Setup Node.js') {
            steps {
                script {
                    def nodeTool = tool name: "nodejs-${NODE_VERSION}", type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
                    env.PATH = "${nodeTool}/bin:${env.PATH}"
                }
            }
        }

        stage('Install Dependencies - Frontend') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                }
            }
        }

        stage('Build Angular App') {
            steps {
                dir('frontend') {
                    sh 'npm run build'
                }
            }
        }

        stage('Run Angular Unit Tests') {
            steps {
                dir('frontend') {
                    sh 'npm run test -- --watch=false --browsers=ChromeHeadless'
                }
            }
        }

        stage('Install Dependencies - Backend') {
            steps {
                dir('backend-fastify') {
                    sh 'npm install'
                }
            }
        }

        stage('Start Fastify Backend') {
            steps {
                dir('backend-fastify') {
                    sh 'nohup node server.js &'
                }
            }
        }

        stage('Archive Angular Build') {
            steps {
                archiveArtifacts artifacts: 'frontend/dist/**/*', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Build and deployment successful! 🎉'
        }
        failure {
            echo 'Build failed. Check logs for issues. ❌'
        }
    }
}
