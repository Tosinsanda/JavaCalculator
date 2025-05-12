pipeline {
    agent any

    environment {
        IMAGE_NAME = 'tosinsanda/java-calc'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/Tosinsanda/JavaCalculator.git'
            }
        }

        stage('Build JAR') {
            steps {
                sh 'docker run --rm -v "$PWD":/usr/src/mymaven -w /usr/src/mymaven maven:3.9-eclipse-temurin-17 mvn clean package -DskipTests'
            }
        }

        stage('Rename JAR') {
            steps {
                sh 'mv target/RaviCalculator-1.4.jar target/app.jar'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-token', variable: 'DOCKER_TOKEN')]) {
                    sh 'echo "$DOCKER_TOKEN" | docker login -u "tosinsanda" --password-stdin'
                    sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
                }
            }
        }
    }
}

