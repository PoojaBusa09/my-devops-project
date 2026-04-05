pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/PoojaBusa09/my-devops-project.git'
'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t my-devops-app .'
            }
        }

        stage('Run Container') {
            steps {
                bat 'docker run -d -p 9090:80 my-devops-app'
            }
        }
    }
}