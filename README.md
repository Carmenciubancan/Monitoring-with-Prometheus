# Project 1: Install Prometheus Stack in Kubernetes

## What This Does

Deploys the complete monitoring infrastructure using Helm.
- **Prometheus** - Collects and stores metrics
- **Grafana** - Visualizes data in dashboards 
- **AlertManager** - Sends notifications when problems occur

## Prerequisites

- Minikube installed
- kubectl installed
- Helm 3 installed

## Deployment Steps

### 1. Start Minikube Cluster
```bash
minikube start
```

### 2. Add Prometheus Helm Repository
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

### 3. Create Monitoring Namespace
```bash
kubectl create namespace monitoring
```

### 4. Install Prometheus Stack
```bash
helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring
```

### 5. Verify Installation
```bash
kubectl get pods -n monitoring

```

## Accessing the UIs

### Access Prometheus UI
```bash
kubectl port-forward svc/monitoring-kube-prometheus-prometheus 9090:9090 -n monitoring &
```

Then open: http://localhost:9090

### Access Grafana
```bash
kubectl port-forward svc/monitoring-grafana 8080:80 -n monitoring &
```

Then open: http://localhost:8080


### Access AlertManager UI
```bash
kubectl port-forward svc/monitoring-kube-prometheus-alertmanager 9093:9093 -n monitoring &
```

Then open: http://localhost:9093
