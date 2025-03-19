pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/patildinu/Angular-live-project'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Angular App') {
            steps {
                sh 'npm run build'
                sh 'ls -R src/dist/' // Debugging: Verify if files are generated
            }
        }

        stage('Archive Build Artifacts') {
            steps {
                archiveArtifacts artifacts: 'src/dist/**/*', fingerprint: true
            }
        }

        stage('Deploy to Nginx') {
            steps {
                sh '''
                sudo yum install -y nginx
                sudo systemctl start nginx
                sudo systemctl enable nginx
                sudo rm -rf /usr/share/nginx/html/*
                sudo cp -r src/dist/* /usr/share/nginx/html/
                sudo systemctl restart nginx
                '''
            }
        }
    }
}







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
