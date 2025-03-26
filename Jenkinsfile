pipeline {
    agent any

    environment {
        PATH = "/usr/bin:/usr/local/bin:$PATH" // Ensure global npm binaries are accessible
        BUILD_DIR = 'dist'
        EC2_USER = "ec2-user"
        EC2_IP = "13.202.85.195"
        REMOTE_DIR = "/usr/share/nginx/html"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/patildinu/real-estate-management'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'node -v'
                sh 'npm -v'
                sh 'npm install @angular/cli'  // Ensure global installation
                sh 'export PATH=$(npm root -g)/.bin:$PATH' // Add npm global binaries to PATH
                sh 'ng version'  // Verify installation
                sh 'rm -rf node_modules package-lock.json || true'
                sh 'npm cache clean --force'
                sh 'npm install'
            }
        }
        stage('Build Angular App') {
            steps {
                sh 'export PATH=$(npm root -g)/.bin:$PATH && ng build --output-path=dist'
                sh 'ls -l ${BUILD_DIR}/'
            }
        }
        stage('Deploy to EC2') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'EC2_SSH_KEY', keyFileVariable: 'SSH_KEY_PATH', usernameVariable: 'SSH_USER')]) {
                    sh "scp -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} -r ${BUILD_DIR}/* ${EC2_USER}@${EC2_IP}:${REMOTE_DIR}/"
                    sh "ssh -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} ${EC2_USER}@${EC2_IP} 'sudo systemctl restart nginx'"
                }
            }
        }
    }
}





// pipeline {
//     agent any

//     environment {
//         PATH = "/usr/bin:$PATH"           // Ensure npm is accessible
//         BUILD_DIR = 'dist'                // Output folder for build artifacts
//         EC2_USER = "ec2-user"
//         EC2_IP = "13.202.85.195"          // Replace with your EC2 public IP
//         REMOTE_DIR = "/usr/share/nginx/html" // Change if your web root is different
//     }

//     stages {
//         stage('Checkout Code') {
//             steps {
//                 git branch: 'master', url: 'https://github.com/patildinu/Angular-live-project'
//             }
//         }
//         stage('Install Dependencies') {
//             steps {
//                 sh 'node -v'
//                 sh 'npm -v'
//                 sh 'npm install @angular/cli'  // Install Angular CLI
//                 sh 'ng version'                   // Verify Angular CLI installation
//                 sh 'rm -rf node_modules package-lock.json || true'
//                 sh 'npm cache clean --force'
//                 sh 'npm install'
//             }
//         }
//         stage('Build Angular App') {
//             steps {
//                 sh 'ng build --output-path=dist'  // Use Angular CLI for building
//                 sh 'ls -l ${BUILD_DIR}/'
//             }
//         }
//         stage('Create Environment Files') {
//             steps {
//                 sh 'echo "Operations Deployment" > od'
//                 sh 'echo "Continuous Integration" > ci'
//                 sh 'echo "Pull Request" > pr'
//                 sh 'ls -l'
//             }
//         }
//         stage('Archive Build Artifacts') {
//             steps {
//                 archiveArtifacts artifacts: "${BUILD_DIR}/**/*, od, ci, pr", fingerprint: true
//             }
//         }
//         stage('Deploy to EC2') {
//             steps {
//                 withCredentials([sshUserPrivateKey(credentialsId: 'EC2_SSH_KEY', keyFileVariable: 'SSH_KEY_PATH', usernameVariable: 'SSH_USER')]) {
//                     script {
//                         def cleanCmd = """
//                         ssh -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} ${EC2_USER}@${EC2_IP} '
//                             sudo rm -rf ${REMOTE_DIR}/* &&
//                             sudo mkdir -p ${REMOTE_DIR} &&
//                             sudo chown ${EC2_USER}:${EC2_USER} ${REMOTE_DIR} &&
//                             exit
//                         '
//                         """
//                         echo "Clean command: ${cleanCmd}"
//                         sh cleanCmd
//                     }
//                     // Copy build artifacts from dist/
//                     sh "scp -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} -r ${BUILD_DIR}/* ${EC2_USER}@${EC2_IP}:${REMOTE_DIR}/"
//                     // Copy additional environment files
//                     sh "scp -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} od ci pr ${EC2_USER}@${EC2_IP}:${REMOTE_DIR}/"
//                     // **New Step**: Copy index.html from src/ (if that's where it is)
//                     sh "scp -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} src/index.html ${EC2_USER}@${EC2_IP}:${REMOTE_DIR}/"
//                     // Copy the styles folder from your repository (adjust path if needed)
//                     sh "scp -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} -r src/styles ec2-user@${EC2_IP}:/usr/share/nginx/html/"
//                     // Restart Nginx on the EC2 instance
//                     sh "ssh -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} ${EC2_USER}@${EC2_IP} 'sudo systemctl restart nginx'"
//                 }
//             }
//         }
//     }
// }




















// pipeline {
//     agent any

//     environment {
//         NODEJS_HOME = tool name: 'NodeJS', type: 'nodejs'
//         PATH = "${NODEJS_HOME}/bin:${env.PATH}"
//     }

//     stages {
//         stage('Checkout Code') {
//             steps {
//                 git branch: 'main', url: 'https://github.com/patildinu/real-estate-management.git'
//             }
//         }

//         stage('Setup Node.js') {
//             steps {
//                 sh 'node -v'  
//                 sh 'npm -v'   
//             }
//         }

//         stage('Install Dependencies') {
//             steps {
//                 dir('frontend') {  // Ensure we're inside the frontend folder
//                     sh 'npm install'
//                 }
//             }
//         }

//         stage('Build Angular App') {
//             steps {
//                 dir('frontend') {
//                     sh 'npm run build -- --configuration=production'
//                 }
//             }
//         }

//         stage('Deploy to Server') {
//             steps {
//                 sh '''
//                 sudo rm -rf /var/www/html/*
//                 sudo cp -r frontend/dist/* /var/www/html/
//                 '''
//             }
//         }
//     }

//     post {
//         success {
//             echo '✅ Angular Build & Deployment Successful!'
//         }
//         failure {
//             echo '❌ Build Failed! Check logs.'
//         }
//     }
// }





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
