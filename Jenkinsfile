pipeline {
    agent any

    environment {
        NODEJS_HOME = tool name: 'NodeJS', type: 'nodejs'
        PATH = "${NODEJS_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/patildinu/real-estate-management.git'
            }
        }

        stage('Setup Node.js') {
            steps {
                sh 'node -v'  // Check Node.js version
                sh 'npm -v'   // Check npm version
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Angular App') {
            steps {
                sh 'npm run build -- --configuration=production'
            }
        }

        stage('Deploy to Server') {
            steps {
                sh '''
                sudo rm -rf /var/www/html/*
                sudo cp -r dist/real-estate-management/frontend* /var/www/html/
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Angular Build & Deployment Successful!'
        }
        failure {
            echo '❌ Build Failed! Check logs.'
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
