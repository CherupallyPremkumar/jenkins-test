pipeline {
    agent {
        docker {
            image 'maven:3.9.9-eclipse-temurin-17'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    environment {
        IMAGE_NAME = "ghcr.io/<GITHUB_USERNAME>/my-java-backend:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Push to GHCR') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'ghcr-cred', usernameVariable: 'GITHUB_USER', passwordVariable: 'GITHUB_TOKEN')]) {
                    sh "echo $GITHUB_TOKEN | docker login ghcr.io -u $GITHUB_USER --password-stdin"
                    sh "docker push ${IMAGE_NAME}"
                }
            }
        }
    }
    
    post {
        always {
            sh "docker rmi ${IMAGE_NAME} || true"
        }
    }
}
