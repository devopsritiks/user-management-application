pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }

        stage('Build Maven (Skip Tests)') {
            steps { sh 'mvn -DskipTests clean package' }
        }

        stage('Build Docker Image') {
            steps { sh 'docker build -t usermanagement-application-image:latest .' }
        }

        stage('Docker Login + Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials',
                 usernameVariable: 'DOCKERHUB_USER',
                 passwordVariable: 'DOCKERHUB_PASS')]) {

                    sh '''
                        echo "$DOCKERHUB_PASS" | docker login -u "$DOCKERHUB_USER" --password-stdin
                        docker tag usermanagement-application-image:latest "$DOCKERHUB_USER/usermanagement:latest"
                        docker push "$DOCKERHUB_USER/usermanagement:latest"
                    '''
                 }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                sh 'docker compose up -d --build'
                sh 'docker ps'
            }
        }

    }
}
