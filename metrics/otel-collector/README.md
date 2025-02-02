# OpenTelemetry Collector Example

This example demonstrates how to use the OpenTelemetry Collector to scrape Prometheus metrics and forward them to Metoro. This is the recommended approach as it better preserves metric type information and provides more flexibility in metric processing.

## Components

1. **Metrics Producer** (`metrics-producer.yaml`)
   - A sample application that produces Prometheus metrics
   - Exposes metrics on port 8080
   - Deployed as a Kubernetes service for Prometheus to scrape

2. **Prometheus**    
   - Required to scrape and collect metrics from the producer
   - The OpenTelemetry Collector will scrape metrics from this Prometheus instance

3. **OpenTelemetry Collector** (`otel-collector.yaml`)
   - Uses the OpenTelemetry Collector Contrib image for advanced features
   - Configured to scrape Prometheus metrics
   - Transforms metrics and forwards them to Metoro

## Prerequisites

- Kubernetes cluster
- Metoro exporter installed in your cluster (namespace: `metoro`)
- `kubectl` configured to access your cluster

## Installation

1. Create the namespace and deploy
```bash
kubectl apply -f ./
```

2. Check that the metrics are being exported to Metoro [here](https://us-east.metoro.io/metric-explorer?chart=%7B%22startTime%22%3A1738455127%2C%22endTime%22%3A1738456027%2C%22metricSpecifiers%22%3A%5B%7B%22visualization%22%3A%7B%22displayName%22%3A%22Tcp+Connections%22%7D%2C%22metricName%22%3A%22otel_exporter_jumpy_gauge%22%2C%22filters%22%3A%7B%22dataType%22%3A%22Map%22%2C%22value%22%3A%5B%5D%7D%2C%22excludeFilters%22%3A%7B%22dataType%22%3A%22Map%22%2C%22value%22%3A%5B%5D%7D%2C%22splits%22%3A%5B%5D%2C%22metricType%22%3A%22metric%22%2C%22functions%22%3A%5B%5D%2C%22aggregation%22%3A%22avg%22%2C%22bucketSize%22%3A0%7D%5D%2C%22type%22%3A%22line%22%7D&startEnd=)


## Advantages Over Remote Write

1. **Better Type Preservation**
   - Maintains counter, histogram, and summary types
   - Preserves metric metadata

2. **Metric Transformation**
   - Built-in support for metric name and label transformation
   - Can filter or modify metrics before sending

## Verification

1. Check that all pods are running:
```bash
kubectl get pods -n prometheus
```

2. Verify Prometheus is scraping the metrics producer:
```bash
kubectl port-forward -n prometheus svc/prometheus 9090:9090
```
Then visit `http://localhost:9090/targets` in your browser

3. View collector logs:
```bash
kubectl logs -n prometheus deployment/otel-collector
```

4. Check your Metoro dashboard for the incoming metrics

## Troubleshooting

1. Check collector logs for errors:
```bash
kubectl logs -n prometheus deployment/otel-collector
```

2. Check Prometheus logs:
```bash
kubectl logs -n prometheus deployment/prometheus
```

3. Verify scraping configuration:
```bash
kubectl get configmap -n prometheus otel-collector-config -o yaml
```

4. Common issues:
   - Ensure Prometheus is running and scraping metrics successfully
   - Ensure the collector has the correct permissions
   - Check that the metrics producer is accessible
   - Verify the Metoro exporter endpoint is correct
