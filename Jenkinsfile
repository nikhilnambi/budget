pipeline {
    agent any

    environment {
        NODEJS_HOME = "/usr/bin"
        PATH = "${NODEJS_HOME}:${env.PATH}"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/nikhilnambi/Budget.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Archive Build Artifacts') {
            steps {
                archiveArtifacts artifacts: 'build/**', fingerprint: true
            }
        }

        stage('Deploy to Docker Container') {
            steps {
                sh '''
                docker build -t react-app .
                docker stop react-app-container || true
                docker rm react-app-container || true
                docker run -d -p 3000:3000 --name react-app-container react-app
                '''
            }
        }
    }
}
