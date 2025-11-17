# True Blue-Green Deployment CI/CD Project

## Overview
This project implements a CI/CD pipeline using Jenkins, AWS ECR, Kubernetes, and Helm for **true blue-green deployments**.

### Components
- **Jenkinsfile**: Automates build, push, deploy, traffic switch, and rollback.
- **Helm Chart**: Creates two deployments (blue and green) and one service.
- **Dockerfile & App**: Simple Flask app.

## How True Blue-Green Works
- Both environments (blue and green) exist simultaneously.
- Service routes traffic to one color (blue or green).
- Pipeline deploys new color, validates, then switches traffic.
- Rollback simply switches service selector back.

## Pipeline Workflow
1. Checkout code.
2. Build Docker image and push to AWS ECR.
3. Detect current live color.
4. Deploy new color using Helm.
5. Health check new color.
6. Switch traffic to new color.

## Helm Usage
```bash
helm upgrade --install finacplus helm/bluegreen --set image.tag=<tag> --set newColor=<color>
```

## Disaster Recovery
Rollback traffic:
```bash
kubectl patch svc finacplus-service -p '{"spec":{"selector":{"color":"blue"}}}'
```

## Monitoring
- Jenkins logs.
- Kubernetes rollout status.
- Recommended: Prometheus + Grafana.
