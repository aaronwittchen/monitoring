# External Scrape Monitoring

Minimal monitoring stack designed for external Prometheus scraping. Deploys only the essential exporters without in-cluster Prometheus/Grafana.

## Components

- **node-exporter**: DaemonSet with hostNetwork (port 9100 on each node)
- **kube-state-metrics**: Deployment with NodePort service (30080)

## Exposed Endpoints

| Component | Port | Type | Description |
|-----------|------|------|-------------|
| node-exporter | 9100 | hostNetwork | Node metrics on each K8s node IP |
| kube-state-metrics | 30080 | NodePort | Kubernetes object metrics |

## Deployment

```bash
# Remove existing kube-prometheus stack first
kubectl delete -k ../longhorn --ignore-not-found

# Deploy minimal exporters
kubectl apply -k .
```

## External Prometheus Scrape Targets

Add these to your external Prometheus:

```yaml
# Node Exporter (all nodes)
- targets:
    - 192.168.68.121:9100  # cp1
    - 192.168.68.122:9100  # cp2
    - 192.168.68.123:9100  # cp3
    - 192.168.68.131:9100  # worker1
    - 192.168.68.132:9100  # worker2
    - 192.168.68.133:9100  # worker3

# Kube State Metrics (any node)
- targets:
    - 192.168.68.121:30080
```

## Migrating from kube-prometheus

1. Delete the kube-prometheus stack
2. Apply this minimal stack
3. Update external Prometheus config
4. Import Grafana dashboards on external Grafana
