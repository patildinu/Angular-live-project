// Build Jenkinsfile: 

pipeline {
    agent any

    environment {
        PATH = "/usr/bin:$PATH"  // Ensure npm is accessible
        BUILD_DIR = 'dist'       // Expected output folder for build artifacts
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/patildinu/Angular-live-project'
            }
        }
        stage('Install Dependencies') {
            steps {
                // Print Node and npm versions for debugging
                sh 'node -v'
                sh 'npm -v'
                // Clean up previous installs
                sh 'rm -rf node_modules package-lock.json || true'
                sh 'npm cache clean --force'
                // Install dependencies freshly
                sh 'npm install'
            }
        }
        stage('Build Angular App') {
            steps {
                // Build the Angular app with output directed to BUILD_DIR
                sh 'npm run build -- --output-path=dist'
                // List the output directory to verify build artifacts
                sh 'ls -l ${BUILD_DIR}/'
            }
        }
        stage('Archive Build Artifacts') {
            steps {
                archiveArtifacts artifacts: "${BUILD_DIR}/**/*", fingerprint: true
            }
        }
    }
}










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
