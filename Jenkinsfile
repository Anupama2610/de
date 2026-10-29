pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "anupama2610/pro"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: ' https://github.com/Anupama2610/de.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE%:latest .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([
                    string(
                        credentialsId: 'dockerhub-pat',
                        variable: 'DOCKER_PAT'
                    )
                ]) {
                    bat '''
                    @echo off
                    echo %DOCKER_PAT% | docker login -u Anupama2610 --password-stdin
                    if errorlevel 1 exit /b 1
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push %DOCKER_IMAGE%:latest'
            }
        }
    }

    post {
        success {
            echo 'Docker image built and pushed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}
