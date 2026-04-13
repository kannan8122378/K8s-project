pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "kannan0607/flask-app"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE%:latest .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS')]) {

                    bat 'docker login -u %USER% -p %PASS%'
                    bat 'docker push %DOCKER_IMAGE%:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
