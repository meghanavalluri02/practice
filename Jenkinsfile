pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'meghanavalluri/simple-java-app:${BUILD_NUMBER}' // Unique versioned image
        DOCKER_CREDENTIALS = 'Dockerhub-creds' // Jenkins Docker Hub Credentials ID
       
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    git branch: 'main', url: 'https://github.com/meghanavalluri02/practice.git'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

      stage('Push Docker Image') {
    steps {
        withCredentials([usernamePassword(credentialsId: "${Dockerhub-creds}", usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
            sh '''
                echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                docker push ${IMAGE_NAME}:latest
            '''
        }
    }
}


         stage('Deploy to Kubernetes') {
            steps {
                script {
                    withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                        sh "kubectl apply -f deployment.yaml"
                        sh "kubectl apply -f service.yaml"
                    }
                }
            }
        }
    }

    post {
        failure {
            echo "Pipeline failed! Check the logs for details."
        }
        success {
            echo "Deployment successful! 🚀"
        }
    }
}
