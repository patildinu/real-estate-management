
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

        stage('Install Dependencies - Frontend') {
            steps {
                dir('frontend') {
                    bat 'npm install'
                }
            }
        }

        stage('Build Angular App') {
            steps {
                dir('frontend') {
                    bat 'npm run build'
                }
            }
        }

        stage('Run Angular Unit Tests') {
            steps {
                dir('frontend') {
                    bat 'npm test'
                }
            }
        }

        stage('Install Dependencies - Backend') {
            steps {
                dir('backend') {
                    bat 'npm install'
                }
            }
        }

        stage('Start Fastify Backend') {
            steps {
                dir('backend') {
                    bat 'npm start'
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
        failure {
            echo 'Build failed. Check logs for issues. ❌'
        }
        success {
            echo 'Build and deployment successful! ✅'
        }
    }
}






//Old code run this script sucessfully



// pipeline {


//     stages {
//         stage('Checkout Code') {
//             steps {
//                 git branch: 'main', url: 'https://github.com/patildinu/real-estate-management.git'
//             }
//         }

//         stage('Setup Node.js') {
//             steps {
//                 script {
//                     def nodeHome = tool name: 'NodeJS', type: 'nodejs'
//                     env.PATH = "${nodeHome}\\bin;${env.PATH}"
//                 }
//             }
//         }

//         stage('Install Dependencies') {
//             steps {
//                 dir('frontend') {  // Change 'frontend' to the correct folder
//                     bat 'npm install'
//                 }
//             }
//         }

//         stage('Run Application') {
//             steps {
//                 dir('frontend') {  // Change 'frontend' to the correct folder
//                     bat 'npm start'
//                 }
//             }
//         }
//     }

//     post {
//         failure {
//             echo 'Build failed. Check logs for issues. ❌'
//         }
//         success {
//             echo 'Build and deployment successful! ✅'
//         }
//     }
// }
