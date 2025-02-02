# Prometheus Remote Write Example

This example demonstrates how to send metrics to Metoro using Prometheus Remote Write protocol. While this method is simpler to set up, note that it may not preserve all metric type information due to limitations in the Prometheus Remote Write protocol.

## Components

1. **Metrics Producer** (`metrics-producer.yaml`)
   - A sample application that produces Prometheus metrics
   - Exposes metrics on port 8080
   - Deployed as a Kubernetes service for Prometheus to scrape

2. **Prometheus with Remote Write** (`prometheus-remote-write.yaml`)
   - Configured to scrape metrics from the sample application
   - Set up to send metrics to Metoro using Remote Write

## Prerequisites

- Kubernetes cluster
- Metoro exporter installed in your cluster (namespace: `metoro`)
- `kubectl` configured to access your cluster

## Installation

1. Create the namespace and deploy
```bash
kubectl apply -f ./
```

2. Check that the metrics are being exported to Metoro [here]([text](https://us-east.metoro.io/metric-explorer?chart=%7B%22startTime%22%3A1738455127%2C%22endTime%22%3A1738456027%2C%22metricSpecifiers%22%3A%5B%7B%22visualization%22%3A%7B%22displayName%22%3A%22Tcp+Connections%22%7D%2C%22metricName%22%3A%22jumpy_gauge%22%2C%22filters%22%3A%7B%22dataType%22%3A%22Map%22%2C%22value%22%3A%5B%5D%7D%2C%22excludeFilters%22%3A%7B%22dataType%22%3A%22Map%22%2C%22value%22%3A%5B%5D%7D%2C%22splits%22%3A%5B%5D%2C%22metricType%22%3A%22metric%22%2C%22functions%22%3A%5B%5D%2C%22aggregation%22%3A%22avg%22%2C%22bucketSize%22%3A0%7D%5D%2C%22type%22%3A%22line%22%7D&startEnd=)