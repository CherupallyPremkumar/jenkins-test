pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-java-backend:latest"
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
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Run Container') {
            steps {
                sh "docker run -d -p 8080:8080 --name java-backend ${DOCKER_IMAGE}"
            }
        }
    }

    post {
        always {
            sh 'docker rm -f java-backend || true'
        }
    }
}
