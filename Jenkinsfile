pipeline {
    agent any

    environment {
        IMAGE_NAME = "meghanavalluri/simple-java-app"
        CREDENTIALS_ID = "dockerhub-creds"
        KUBECONFIG = "/var/lib/jenkins/.kube/config"
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
                    dockerImage = docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', CREDENTIALS_ID) {
                        dockerImage.push("latest")
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withEnv(["KUBECONFIG=${KUBECONFIG}"]) {
                    sh 'kubectl apply -f deployment.yaml'
                }
            }
        }
    }
}
