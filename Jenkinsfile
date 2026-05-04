pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "tominsahu/devops-demo"
        TAG = "latest"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/Tomin-sahu/devops-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${TAG} ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push ${DOCKER_IMAGE}:${TAG}"
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh "kubectl apply -f k8s.yaml"
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Pipeline Failed!'
        }
    }
}
