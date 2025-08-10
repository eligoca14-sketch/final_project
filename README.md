# Microservices Example - Improved

## How to run locally (Docker Compose)
1. docker-compose up --build
2. Visit http://localhost:5000/users and http://localhost:5001/items
3. Prometheus UI: http://localhost:9090

## How to run on Kubernetes (Kind)
The GitHub Actions workflow demonstrates how to run this on a Kind cluster. Locally you can:
1. Install kind and kubectl.
2. Create a cluster: `kind create cluster`
3. Build and load images: `docker build -t user-service:latest ./user-service && kind load docker-image user-service:latest`
4. Apply manifests: `kubectl apply -f k8s/`
