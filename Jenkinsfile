pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "tominsahu/devops-demo"
        TAG = "latest"
        KUBECONFIG = "C:\\Users\\<your-username>\\.kube\\config"
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
                    credentialsId: 'docker-cred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
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

        stage('Check Kubernetes Connection') {
            steps {
                bat "kubectl cluster-info"
                bat "kubectl get nodes"
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
