pipeline {
    agent any

    environment {
        PATH = "/usr/bin:$PATH"           // Ensure npm is accessible
        BUILD_DIR = 'dist'                // Output folder for build artifacts
        EC2_USER = "ec2-user"
        EC2_IP = "13.202.85.195"          // Replace with your EC2 public IP
        REMOTE_DIR = "/usr/share/nginx/html" // Change if your web root is different
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/patildinu/Angular-live-project'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'node -v'
                sh 'npm -v'
                sh 'npm install -g @angular/cli'  // Install Angular CLI
                sh 'ng version'                   // Verify Angular CLI installation
                sh 'rm -rf node_modules package-lock.json || true'
                sh 'npm cache clean --force'
                sh 'npm install'
            }
        }
        stage('Build Angular App') {
            steps {
                sh 'ng build --output-path=dist'  // Use Angular CLI for building
                sh 'ls -l ${BUILD_DIR}/'
            }
        }
        stage('Create Environment Files') {
            steps {
                sh 'echo "Operations Deployment" > od'
                sh 'echo "Continuous Integration" > ci'
                sh 'echo "Pull Request" > pr'
                sh 'ls -l'
            }
        }
        stage('Archive Build Artifacts') {
            steps {
                archiveArtifacts artifacts: "${BUILD_DIR}/**/*, od, ci, pr", fingerprint: true
            }
        }
        stage('Deploy to EC2') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'EC2_SSH_KEY', keyFileVariable: 'SSH_KEY_PATH', usernameVariable: 'SSH_USER')]) {
                    script {
                        def cleanCmd = """
                        ssh -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} ${EC2_USER}@${EC2_IP} '
                            sudo rm -rf ${REMOTE_DIR}/* &&
                            sudo mkdir -p ${REMOTE_DIR} &&
                            sudo chown ${EC2_USER}:${EC2_USER} ${REMOTE_DIR} &&
                            exit
                        '
                        """
                        echo "Clean command: ${cleanCmd}"
                        sh cleanCmd
                    }
                    // Copy build artifacts from dist/
                    sh "scp -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} -r ${BUILD_DIR}/* ${EC2_USER}@${EC2_IP}:${REMOTE_DIR}/"
                    // Copy additional environment files
                    sh "scp -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} od ci pr ${EC2_USER}@${EC2_IP}:${REMOTE_DIR}/"
                    // **New Step**: Copy index.html from src/ (if that's where it is)
                    sh "scp -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} src/index.html ${EC2_USER}@${EC2_IP}:${REMOTE_DIR}/"
                    // Copy the styles folder from your repository (adjust path if needed)
                    sh "scp -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} -r src/styles ec2-user@${EC2_IP}:/usr/share/nginx/html/"
                    // Restart Nginx on the EC2 instance
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
//                 sh 'rm -rf node_modules package-lock.json || true'
//                 sh 'npm cache clean --force'
//                 sh 'npm install'
//             }
//         }
//         stage('Build Angular App') {
//             steps {
//                 sh 'npm run build -- --output-path=dist'
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












// // Build Jenkinsfile: 

// pipeline {
//     agent any

//     environment {
//         PATH = "/usr/bin:$PATH"  // Ensure npm is accessible
//         BUILD_DIR = 'dist'       // Expected output folder for build artifacts
//     }

//     stages {
//         stage('Checkout Code') {
//             steps {
//                 git branch: 'master', url: 'https://github.com/patildinu/Angular-live-project'
//             }
//         }
//         stage('Install Dependencies') {
//             steps {
//                 // Print Node and npm versions for debugging
//                 sh 'node -v'
//                 sh 'npm -v'
//                 // Clean up previous installs
//                 sh 'rm -rf node_modules package-lock.json || true'
//                 sh 'npm cache clean --force'
//                 // Install dependencies freshly
//                 sh 'npm install'
//             }
//         }
//         stage('Build Angular App') {
//             steps {
//                 // Build the Angular app with output directed to BUILD_DIR
//                 sh 'npm run build -- --output-path=dist'
//                 // List the output directory to verify build artifacts
//                 sh 'ls -l ${BUILD_DIR}/'
//             }
//         }
//         stage('Archive Build Artifacts') {
//             steps {
//                 archiveArtifacts artifacts: "${BUILD_DIR}/**/*", fingerprint: true
//             }
//         }
//     }
// }























// pipeline {
//     agent any

//     stages {
//         stage('Checkout Code') {
//             steps {
//                 git 'https://github.com/patildinu/Angular-live-project'
//             }
//         }

//         stage('Install Dependencies') {
//             steps {
//                 sh 'npm install'
//             }
//         }

//         stage('Build Angular App') {
//             steps {
//                 sh 'npm run build'
//                 sh 'ls -R src/dist/' // Debugging: Verify if files are generated
//             }
//         }

//         stage('Archive Build Artifacts') {
//             steps {
//                 archiveArtifacts artifacts: 'src/dist/**/*', fingerprint: true
//             }
//         }

//         stage('Deploy to Nginx') {
//             steps {
//                 sh '''
//                 # Install Nginx if not installed
//                 sudo yum install -y nginx || true

//                 # Ensure Nginx directory exists
//                 sudo mkdir -p /usr/share/nginx/html/

//                 # Change ownership so Jenkins can copy files
//                 sudo chown -R jenkins:jenkins /usr/share/nginx/html/

//                 # Remove old files
//                 sudo rm -rf /usr/share/nginx/html/*

//                 # Copy build files to Nginx directory
//                 sudo cp -r src/dist/* /usr/share/nginx/html/

//                 # Set proper permissions
//                 sudo chown -R nginx:nginx /usr/share/nginx/html/

//                 # Restart Nginx to apply changes
//                 sudo systemctl restart nginx
//                 sudo systemctl enable nginx
//                 '''
//             }
//         }
//     }
// }










// pipeline {
//     agent any

//     stages {
//         stage('Checkout Code') {
//             steps {
//                 git 'https://github.com/patildinu/Angular-live-project'
//             }
//         }

//         stage('Install Dependencies') {
//             steps {
//                 sh 'npm install'
//             }
//         }

//         stage('Build Angular App') {
//             steps {
//                 sh 'npm run build'
//                 sh 'ls -R src/dist/' // Debugging: Verify if files are generated
//             }
//         }

//         stage('Archive Build Artifacts') {
//             steps {
//                 archiveArtifacts artifacts: 'src/dist/**/*', fingerprint: true
//             }
//         }

//         stage('Deploy to Nginx') {
//             steps {
//                 sh '''
//                 sudo yum install -y nginx
//                 sudo systemctl start nginx
//                 sudo systemctl enable nginx
//                 sudo rm -rf /usr/share/nginx/html/*
//                 sudo cp -r src/dist/* /usr/share/nginx/html/
//                 sudo systemctl restart nginx
//                 '''
//             }
//         }
//     }
// }







// Old code running 


// pipeline {
//     agent any

//     stages {
//         stage('Checkout Code') {
//             steps {
//                 git 'https://github.com/patildinu/Angular-live-project'
//             }
//         }

//         stage('Install Dependencies') {
//             steps {
//                 sh 'npm install'
//             }
//         }

//         stage('Build Angular App') {
//             steps {
//                 sh 'npm run build'
//                 sh 'ls -R src/dist/' // Debugging: Verify if files are generated
//             }
//         }

//         stage('Archive Build Artifacts') {
//             steps {
//                 archiveArtifacts artifacts: 'src/dist/**/*', fingerprint: true
//             }
//         }
//     }
// }
