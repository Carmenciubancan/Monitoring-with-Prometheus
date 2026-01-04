# Project 2: Configure Alerting for Our Application

## What This Does

Configures the monitoring stack to **send alerts** when problems occur. 
- CPU usage exceeds 50%
- A Pod crashes or can't start


## Files in This Project

- **alert-rules.yaml** - Defines Prometheus alert rules (when to trigger alerts)
- **alertmanager-config.yaml** - Configures AlertManager email notifications

## How It Works

**Prometheus** watches metrics → **Alert rules** check thresholds → **AlertManager** sends notifications

## Deployment Steps

### 1. Apply Alert Rules
```bash
kubectl apply -f alert-rules.yaml -n monitoring
```

This creates a `PrometheusRule` custom resource that Prometheus automatically detects and loads.

### 2. Configure AlertManager for Email Notifications
```bash
kubectl apply -f alertmanager-config.yaml -n monitoring
```

### 3. Verify Alert Rules are Loaded
```bash
# Check PrometheusRule was created
kubectl get prometheusrule -n monitoring

# Access Prometheus UI and check alerts
kubectl port-forward svc/monitoring-kube-prometheus-prometheus 9090:9090 -n monitoring &
```

Go to http://localhost:9090/alerts to see configured alerts.

### 4. Test Alerts by Triggering CPU Spike

Create a CPU stress test pod:
```bash
kubectl run cpu-test --image=containerstack/cpustress -- --cpu 4 --timeout 60s --metrics-brief
```