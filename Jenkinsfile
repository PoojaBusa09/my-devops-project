pipeline {
    agent any

    environment {
        IMAGE_NAME = 'busapooja/webapp'
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
        withCredentials([usernamePassword(
            credentialsId: 'docker-hub-creds',
            usernameVariable: 'DOCKER_USER',
            passwordVariable: 'DOCKER_PASS'
        )]) {
            bat """
            docker login -u %DOCKER_USER% -p %DOCKER_PASS%
            """
        }
    }
}

stage('Push to DockerHub') {
            steps {
                bat '''
                echo Pushing image...
                docker push %IMAGE_NAME%
                '''
            }
        }

        stage('Remove Old Container') {
            steps {
                bat '''
                echo Removing old container if exists...
                docker rm -f %CONTAINER_NAME% >nul 2>&1
                '''
            }
        }

        stage('Run Container') {
            steps {
                bat '''
                echo Running container...
                docker run -d -p 5000:80 --name %CONTAINER_NAME% %IMAGE_NAME%
                '''
            }
        }
    }
}
