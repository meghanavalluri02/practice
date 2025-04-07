pipeline {
    agent any

    environment {
        IMAGE_NAME = "meghanavalluri/simple-java-app"
        CREDENTIALS_ID = "dockerhub-credentials"
        KUBECONFIG = "/var/lib/jenkins/.kube/config" // Make sure Jenkins has access
    }

    stages {
        stage('Build with Maven') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${CREDENTIALS_ID}") {
                        docker.image("${IMAGE_NAME}").push("latest")
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
