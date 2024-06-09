pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'localhost:5000'
        IMAGE_NAME = 'saba-express-app'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    sh "docker push ${DOCKER_REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Publish') {
            steps {
                script {
                    sh 'kubectl apply -f deployment.yaml'
                    sh 'kubectl apply -f service.yaml'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
