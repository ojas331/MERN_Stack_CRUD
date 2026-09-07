pipeline {
    agent any

    stages {
        stage('Install Backend Dependencies') {
            steps {
                dir('backend') {
                    sh 'npm install'
                }
            }
        }

        stage('Install & Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Deploy with PM2') {
            steps {
                sh 'echo "MONGO_URI=mongodb://127.0.0.1:27017/MERN_CRUD" > backend/.env'
                sh '/usr/local/bin/pm2 delete mern-backend || true'
                sh '/usr/local/bin/pm2 delete mern-frontend || true'
                dir('backend') {
                    sh '/usr/local/bin/pm2 start index.js --name mern-backend'
                }
                sh '/usr/local/bin/pm2 serve frontend/dist 3000 --name mern-frontend --spa'
                sh '/usr/local/bin/pm2 save'
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful! Frontend: http://192.168.0.112:3000'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
