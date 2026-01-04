# Project 3: Monitor Third-Party Application

## The Concept

**Third-party applications** (databases, message queues, web servers) don't expose metrics in Prometheus format natively.

**Solution:** Use a **Prometheus Exporter** - a translator that:
1. Connects to the third-party app
2. Collects its metrics
3. Exposes them in Prometheus format
4. Gets scraped by Prometheus

**Flow:** Third-Party App → Exporter → Prometheus → Grafana


## Redis Database

## Files

- **redis-deployment.yaml** - Deploys Redis database
- **redis-values.yaml** - Configures Redis exporter

## Deployment Steps

### 1. Deploy Redis
```bash
kubectl apply -f redis-deployment.yaml
kubectl get pods -l app=redis
```

### 2. Deploy Redis Exporter
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install redis-exporter prometheus-community/prometheus-redis-exporter -f redis-values.yaml
```

### 3. Verify Metrics in Prometheus
```bash
kubectl port-forward svc/monitoring-kube-prometheus-prometheus 9090:9090 -n monitoring &
```

Go to http://localhost:9090 → search for `redis_` metrics

### 4. Visualize in Grafana
```bash
kubectl port-forward svc/monitoring-grafana 8080:80 -n monitoring &
```