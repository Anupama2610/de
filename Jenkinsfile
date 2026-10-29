pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "anupama2610/prog10"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'YOUR_GITHUB_REPOSITORY_URL'
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
                    echo %DOCKER_PAT% | docker login -u YOUR_DOCKERHUB_USERNAME --password-stdin
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
