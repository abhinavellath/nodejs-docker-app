pipeline {
    agent any

    stages {
        
        stage ("Clone") {
            steps {
                echo "Cloning the code"
                git url: 'https://github.com/abhinavellath/nodejs-docker-app.git', branch: 'main'
                echo "Cloning successful"
            }
        }

        stage ("Build") {
            steps {
                echo "Building the Docker image"
                sh "docker build -t abhinavellath/nodejs-login-app:latest ."
                echo "Build successful"
            }
        }
        
        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds', 
                    usernameVariable: 'DOCKER_USER', 
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                sh 'docker push abhinavellath/nodejs-login-app:latest'
            }
        }

        stage ("Deploy") {
            steps {
                echo "Deploying the application"
                sh "docker-compose up -d"
            }
        }

    }
}
