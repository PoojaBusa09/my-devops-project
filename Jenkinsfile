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

        stage('Remove Old Container') {
            steps {
                bat "docker rm -f %CONTAINER_NAME% || exit 0"
            }
        }

        stage('Run Container') {
            steps {
                bat "docker run -d -p 9090:80 --name %CONTAINER_NAME% %IMAGE_NAME%"
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'docker', url: 'https://index.docker.io/v1/']) {
                        bat "docker push %IMAGE_NAME%"
                    }
                }
            }
        }
    }
}