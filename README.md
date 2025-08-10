# Microservices Example - Improved

## What I added
- Improved Bootstrap frontend for both services with consistent styling and a /metrics link.
- Dockerfiles updated to use a non-root user and multi-stage pattern.
- pytest unit tests for both services.
- Kubernetes manifests (Deployments, Services, HPA, Prometheus & Grafana) in `k8s/`.
- GitHub Actions workflow to run tests, build images, create a Kind cluster, load images, and deploy manifests.
- Updated docker-compose build contexts (already fixed).

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

## Presentation outline
- Architecture diagram (describe services, API endpoints, and Prometheus/Grafana)
- Demo: show services, call /users and /items, show /metrics, show Prometheus target scraping and Grafana dashboard
- CI/CD: explain GitHub Actions workflow and Kind deployment
- Challenges & learnings


## Grafana provisioning and dashboard (added)
I added Grafana provisioning via Kubernetes ConfigMaps:
- `k8s/grafana-provisioning-configmap.yaml` provides the Prometheus datasource and dashboard provider configuration.
- `k8s/grafana-dashboards-configmap.yaml` includes a sample dashboard `microservices-dashboard.json` which graphs `rate(item_service_requests_total[1m])` and `rate(user_service_requests_total[1m])`.
- `k8s/grafana-deployment.yaml` mounts these configmaps into the Grafana container so the datasource and dashboard are auto-provisioned on start.

To see the dashboard after deploying to Kind/Minikube:
1. Deploy k8s manifests: `kubectl apply -f k8s/`
2. Port-forward Grafana: `kubectl port-forward svc/grafana 3000:3000`
3. Visit: http://localhost:3000 (user: admin, password: admin)
4. The dashboard "Microservices Metrics" should appear automatically (or check Manage → Dashboards).

