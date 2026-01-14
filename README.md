# Kubernetes Monitoring Stack

GitOps-friendly monitoring stack based on [kube-prometheus](https://github.com/prometheus-operator/kube-prometheus).

## Components

- **Prometheus Operator** - Manages Prometheus/Alertmanager instances
- **Prometheus** - Metrics collection and storage
- **Grafana** - Dashboards and visualization
- **Alertmanager** - Alert routing and notifications
- **node-exporter** - Host metrics
- **kube-state-metrics** - Kubernetes object metrics

## Structure

```
monitoring/
├── base/
│   ├── kustomization.yaml    # References kube-prometheus v0.14.0
│   └── httproutes.yaml       # Istio Gateway routes
└── overlays/
    └── longhorn/
        ├── kustomization.yaml # Adds persistent storage
        └── grafana-pvc.yaml
```

## URLs

| Service | URL |
|---------|-----|
| Grafana | https://grafana.k8s.local |
| Prometheus | https://prometheus.k8s.local |
| Alertmanager | https://alertmanager.k8s.local |

## Deployment

```bash
# Via ArgoCD (recommended)
kubectl apply -f ArgoCD/applications/monitoring.yaml

# Manual
kubectl apply -k overlays/longhorn
```

## Default Credentials

- **Grafana**: admin / admin (change on first login)

## Storage

Uses Longhorn for persistent storage:
- Prometheus: 10Gi (15 day retention)
- Alertmanager: 1Gi
- Grafana: 2Gi
