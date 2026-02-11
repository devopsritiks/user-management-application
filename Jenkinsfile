pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }

        stage('Build Maven (Skip Tests)') {
            setps { sh 'mvn -DskipTests clean package' }
        }

        stage('Build Docker Image') {
            steps { sh 'docekr build -t usermanagement-application-image:latest .' }
        }

        stage('Deploy with Docker Compose') {
            steps {
                sh 'docker compose up -d --build'
                sh 'docker ps'
            }
        }

    }
}
