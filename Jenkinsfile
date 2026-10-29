pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'seran4595'
        IMAGE_NAME = "${DOCKERHUB_USER}/coupon-app"
        GITHUB_REPO = 'https://github.com/seran4595/coupon-system.git'
        // Explicitly set PATH to include Docker and Kubectl executables
        PATH = "C:\\Program Files\\Docker\\Docker\\resources\\bin;C:\\ProgramData\\DockerDesktop\\version-bin;${env.PATH}"
    }

    stages {
        stage('Build v1') {
            steps {
                git branch: 'v1', url: "${GITHUB_REPO}"
                bat "\"C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe\" build -t ${IMAGE_NAME}:v1 ."
            }
        }

        stage('Build v2') {
            steps {
                git branch: 'v2', url: "${GITHUB_REPO}"
                bat "\"C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe\" build -t ${IMAGE_NAME}:v2 ."
            }
        }

        stage('Build v3') {
            steps {
                git branch: 'v3', url: "${GITHUB_REPO}"
                bat "\"C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe\" build -t ${IMAGE_NAME}:v3 ."
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    bat "\"C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe\" login -u %USER% -p %PASS%"
                    bat "\"C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe\" push ${IMAGE_NAME}:v1"
                    bat "\"C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe\" push ${IMAGE_NAME}:v2"
                    bat "\"C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe\" push ${IMAGE_NAME}:v3"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat "\"C:\\Program Files\\Docker\\Docker\\resources\\bin\\kubectl.exe\" apply -f k8s-deployments.yaml"
            }
        }
    }
}