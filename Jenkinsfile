pipeline {
    agent any

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/YOUR_USERNAME/my-devops-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-devops-app .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8080:80 my-devops-app'
            }
        }
    }
}