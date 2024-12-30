# Spring Boot Demo Application

This is a simple Spring Boot application with CI/CD setup for both Jenkins and Azure DevOps.

## Prerequisites

- Java 17
- Maven
- Docker
- Jenkins (for Jenkins pipeline)
- Azure DevOps account (for Azure pipeline)

## Local Development

1. Build the application:
   ```bash
   ./mvnw clean package
   ```

2. Run the application:
   ```bash
   ./mvnw spring-boot:run
   ```

3. Access the application:
   - Open http://localhost:8080

## Docker Build

1. Build the Docker image:
   ```bash
   docker build -t yourdockerhub/demo-app:latest .
   ```

2. Run the container:
   ```bash
   docker run -p 8080:8080 yourdockerhub/demo-app:latest
   ```

## CI/CD Setup

### Jenkins Pipeline Setup

1. Install required Jenkins plugins:
   - Docker Pipeline
   - Docker
   - Pipeline
   - Git

2. Create Jenkins credentials:
   - Add Docker Hub credentials with ID 'docker-hub-credentials'

3. Create a new Pipeline job:
   - Point to your repository
   - Use the Jenkinsfile from the repository

### Azure DevOps Setup

1. Create a new pipeline:
   - Connect to your repository
   - Use the existing azure-pipelines.yml

2. Configure service connections:
   - Create Docker Hub service connection named 'docker-hub-connection'

3. Run the pipeline

## Deployment

### Kubernetes Deployment

1. Create a deployment:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: demo-app
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: demo-app
     template:
       metadata:
         labels:
           app: demo-app
       spec:
         containers:
         - name: demo-app
           image: yourdockerhub/demo-app:latest
           ports:
           - containerPort: 8080
   ```

2. Apply the deployment:
   ```bash
   kubectl apply -f deployment.yaml
   ```

### Manual Deployment

1. Pull the Docker image:
   ```bash
   docker pull yourdockerhub/demo-app:latest
   ```

2. Run the container:
   ```bash
   docker run -d -p 8080:8080 yourdockerhub/demo-app:latest
   ```

## Testing

Run tests using Maven:
```bash
./mvnw test
```

## Important Notes

- Replace 'yourdockerhub' with your actual Docker Hub username
- Ensure all required credentials are properly configured in Jenkins/Azure DevOps
- The application runs on port 8080 by default
- Make sure to update the Docker image tag strategy according to your needs