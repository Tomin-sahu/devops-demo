pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "tominsahu/devops-demo"
        TAG = "latest"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %DOCKER_IMAGE%:%TAG% ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'sahutomin94@gmail.com',
                    usernameVariable: 'tominsahu',
                    passwordVariable: 'Nseit$2022'
                )]) {
                    bat "echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat "docker push %DOCKER_IMAGE%:%TAG%"
            }
        }

        stage('Deploy to Minikube') {
            steps {
                bat "kubectl apply -f k8s.yaml"
            }
        }
    }

    post {
        success {
            echo 'Pipeline Successful!'
        }
        failure {
            echo 'Pipeline Failed!'
        }
    }
}
