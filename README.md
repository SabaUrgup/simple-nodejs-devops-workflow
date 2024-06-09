# Simple Node.js DevOps Workflow

A simple Node.js application with a complete DevOps workflow, including version control, containerization, orchestration, CI/CD pipeline, and monitoring.

## Steps

1. **Setup and Version Control**
    - Initialize a Git repository and commit your code.
    - Push to a remote repository (GitHub/GitLab).

2. **Dockerization**
    - Create a Dockerfile:
      ```Dockerfile
      FROM node:20
      WORKDIR /usr/src/app
      COPY package*.json ./
      RUN npm install
      COPY . .
      CMD ["node", "app.js"]
      ```
    - Build and run the Docker container:
      ```sh
      docker build -t simple-nodejs-app .
      docker run -p 3000:3000 simple-nodejs-app
      ```

3. **Kubernetes Deployment**
    - Create Kubernetes manifests (`deployment.yaml` and `service.yaml`).
    - Deploy to Minikube:
      ```sh
      kubectl apply -f deployment.yaml
      kubectl apply -f service.yaml
      ```

4. **CI/CD Pipeline**
    - Create a `Jenkinsfile`
    - Push the Jenkinsfile to the repository.

5. **Monitoring**
    - Set up Prometheus and Grafana to monitor the application.

## Getting Started

Clone the repository and follow the steps above to set up and deploy the application.

```sh
git clone https://github.com/SabaUrgup/simple-nodejs-devops-workflow.git
cd simple-nodejs-devops-workflow
