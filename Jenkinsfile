pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/patildinu/real-estate-management.git'
            }
        }

        stage('Setup Node.js') {
            steps {
                script {
                    def nodeHome = tool name: 'NodeJS', type: 'nodejs'
                    env.PATH = "${nodeHome}\\bin;${env.PATH}"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                dir('frontend') {  // Change 'frontend' to the correct folder
                    bat 'npm install'
                }
            }
        }

        stage('Run Application') {
            steps {
                dir('frontend') {  // Change 'frontend' to the correct folder
                    bat 'npm start'
                }
            }
        }
    }

    post {
        failure {
            echo 'Build failed. Check logs for issues. ❌'
        }
        success {
            echo 'Build and deployment successful! ✅'
        }
    }
}
