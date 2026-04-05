pipeline {
    agent any

    environment {
        IMAGE_NAME = 'poojabusa/my-devops-app'
        CONTAINER_NAME = 'my-devops-container'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/PoojaBusa09/my-devops-project.git',
                    credentialsId: 'github-creds'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %IMAGE_NAME% ."
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    bat "echo %PASS% | docker login -u %USER% --password-stdin"
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                bat "docker push %IMAGE_NAME%"
            }
        }

        stage('Remove Old Container') {
            steps {
                bat "docker rm -f %CONTAINER_NAME% || exit 0"
            }
        }

        stage('Run Container') {
            steps {
                bat "docker run -d -p 8080:80 --name %CONTAINER_NAME% %IMAGE_NAME%"
            }
        }
    }
}