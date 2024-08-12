pipeline {
    agent any

    tools {
        nodejs 'node-22.5.1'  // Ensure this matches the name in Global Tool Configuration
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/coding-jaguar/demo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install project dependencies
                sh 'npm install'
                sh 'npm install pm2 -g'
            }
        }



        stage('Deploy') {
            steps {
                script {
                    // Start your application using pm2 or any other process manager
                    sh 'pm2 start index.js --name "express-app"'
                    // // Optionally, you can use pm2 to restart the app instead
                    // sh 'pm2 logs'
                }
            }
        }

        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
                input message: 'Finished using the web site? (Click "Proceed" to continue)' 
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }

    post {
      
        success {
            echo 'Deployment successful!'
        }

        failure {
            echo 'Deployment failed.'
        }
    }
}
