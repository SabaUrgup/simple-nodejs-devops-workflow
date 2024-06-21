# Simple Node.js DevOps Workflow

This repository contains a simple Node.js application with a complete DevOps workflow, including version control, containerization, orchestration, CI/CD pipeline, and monitoring.

## Table of Contents

1. [Steps](#steps)
   - [Setup and Version Control](#setup-and-version-control)
   - [Dockerization](#dockerization)
   - [Kubernetes Deployment](#kubernetes-deployment)
   - [CI/CD Pipeline](#cicd-pipeline)
   - [Monitoring](#monitoring)
2. [Getting Started](#getting-started)
3. [License](#license)

## Steps

### Setup and Version Control

1. **Initialize a Git repository and commit your code**:
    ```sh
    git init
    git add .
    git commit -m "Initial commit: Basic Node.js application setup"
    ```
2. **Push to a remote repository (GitHub/GitLab)**:
    ```sh
    git remote add origin https://github.com/SabaUrgup/simple-nodejs-devops-workflow.git
    git push -u origin master
    ```

### Dockerization

1. **Create a Dockerfile**:
    ```Dockerfile
    FROM node:20
    WORKDIR /usr/src/app
    COPY package*.json ./
    RUN npm install
    COPY . .
    CMD ["node", "app.js"]
    ```
    - This Dockerfile sets up a Node.js environment, installs dependencies, copies the application code, and starts the application.

2. **Build and run the Docker container**:
    ```sh
    docker build -t saba-express-app .
    docker run -p 3000:3000 saba-express-app
    ```
    - This builds a Docker image named `saba-express-app` and runs a container exposing it on port 3000.

### Kubernetes Deployment

1. **Set up Minikube**:
    ```sh
    minikube start
    ```
    - Minikube sets up a local Kubernetes cluster for development and testing.

2. **Create Kubernetes manifests** (`deployment.yaml` and `service.yaml`):

    **deployment.yaml**
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: saba-express-app
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: saba-express-app
      template:
        metadata:
          labels:
            app: saba-express-app
        spec:
          containers:
          - name: saba-express-app
            image: saba-express-app:latest
            ports:
            - containerPort: 3000
    ```

    **service.yaml**
    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: saba-express-app-service
    spec:
      selector:
        app: saba-express-app
      ports:
        - protocol: TCP
          port: 80
          targetPort: 3000
      type: LoadBalancer
    ```

3. **Deploy to Minikube**:
    ```sh
    kubectl apply -f deployment.yaml
    kubectl apply -f service.yaml
    ```
    - These commands apply the deployment and service configurations to the Kubernetes cluster, deploying the application and exposing it via a load balancer.

### CI/CD Pipeline

1. **Set up Jenkins**:
    - Install Jenkins on your local machine or Minikube cluster:
      ```sh
      kubectl create namespace jenkins
      helm repo add jenkins https://charts.jenkins.io
      helm repo update
      helm install jenkins jenkins/jenkins --namespace jenkins
      ```

2. **Create a Jenkinsfile**:
    ```groovy
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
    ```
    - This Jenkinsfile defines a pipeline with stages for building, testing, and deploying the application.

3. **Push the Jenkinsfile to the repository**:
    ```sh
    git add Jenkinsfile
    git commit -m "Add Jenkins pipeline script for CI/CD automation"
    git push
    ```

### Monitoring

1. **Set up Prometheus and Grafana** using Helm charts or Kubernetes manifests:
    ```sh
    kubectl create namespace monitoring
    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    helm repo add grafana https://grafana.github.io/helm-charts
    helm repo update
    helm install prometheus prometheus-community/kube-prometheus-stack --namespace monitoring
    helm install grafana grafana/grafana --namespace monitoring
    ```

2. **Configure Prometheus** to scrape metrics from your application.

3. **Create a Grafana dashboard** to visualize the metrics.

## Getting Started

Clone the repository and follow the steps above to set up and deploy the application.

```sh
git clone https://github.com/SabaUrgup/simple-nodejs-devops-workflow.git
cd simple-nodejs-devops-workflow
```

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
