pipeline {
    agent any
    tools {
        nodejs 'NodeJS'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: '', branch: 'master'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Project') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Running Test Cases') {
            steps {
                sh 'npm run test'
            }
        }

        stage('Build & Deploy Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh """
                    echo Building Docker image...
                    docker build -t dhiraj2001/nodejs-jenkins-app .

                    echo Logging into Docker Hub...
                    echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin

                    echo Pushing Docker image...
                    docker push dhiraj2001/nodejs-jenkins-app

                    echo Logging out...
                    docker logout
                    """
                }
            }
        }
    }
}