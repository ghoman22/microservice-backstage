# Deployment Guide

This guide covers deploying My Microservice to various environments.

## AWS EKS Deployment

### Prerequisites

- AWS CLI configured
- kubectl configured for your EKS cluster
- Docker registry access (ECR recommended)
- EKS cluster with appropriate node groups

### Step 1: Build and Push Image

```bash
# Build the image
docker build -t my-microservice:v1.0.0 .

# Tag for ECR
docker tag my-microservice:v1.0.0 123456789012.dkr.ecr.us-west-2.amazonaws.com/my-microservice:v1.0.0

# Push to ECR
aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-west-2.amazonaws.com
docker push 123456789012.dkr.ecr.us-west-2.amazonaws.com/my-microservice:v1.0.0
```

### Step 2: Deploy to Kubernetes

```bash
# Apply manifests
kubectl apply -f k8s/

# Verify deployment
kubectl get pods -l app=my-microservice
kubectl get svc my-microservice-service
```

### Step 3: Configure Monitoring

```bash
# Deploy monitoring stack
kubectl apply -f monitoring/
```

## Environment-Specific Configurations

### Development

```yaml
# k8s/overlays/dev/kustomization.yaml
resources:
- ../../base
patchesStrategicMerge:
- deployment-patch.yaml
```

### Staging

```yaml
# k8s/overlays/staging/kustomization.yaml
resources:
- ../../base
replicas:
- name: my-microservice
  count: 2
```

### Production

```yaml
# k8s/overlays/prod/kustomization.yaml
resources:
- ../../base
replicas:
- name: my-microservice
  count: 5
```

## Monitoring Setup

### Prometheus Configuration

```yaml
# monitoring/servicemonitor.yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: my-microservice
spec:
  selector:
    matchLabels:
      app: my-microservice
  endpoints:
  - port: http
    path: /metrics
```

### Grafana Dashboard

Import the dashboard from `monitoring/grafana-dashboard.json`.

## Troubleshooting

### Common Issues

1. **Pod not starting**: Check resource limits and image availability
2. **Health checks failing**: Verify endpoints and network policies
3. **Database connection issues**: Check secrets and network connectivity