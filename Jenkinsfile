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
                withCredentials([usernamePassword(
                    credentialsId: 'docker-hub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    bat '''
                    echo %PASS% > pass.txt
                    docker login -u %USER% --password-stdin < pass.txt
                    del pass.txt
                    '''
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
                bat '''
                docker ps -a -q --filter "name=%CONTAINER_NAME%" > container.txt
                set /p CONTAINER_ID=<container.txt
                if not "%CONTAINER_ID%"=="" docker rm -f %CONTAINER_NAME%
                del container.txt
                '''
            }
        }

        stage('Run Container') {
            steps {
                bat '''
                docker run -d -p 8080:80 --name %CONTAINER_NAME% %IMAGE_NAME%
                '''
            }
        }
    }
}
