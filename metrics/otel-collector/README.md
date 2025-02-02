# OpenTelemetry Collector Example

This example demonstrates how to use the OpenTelemetry Collector to scrape Prometheus metrics and forward them to Metoro. This is the recommended approach as it better preserves metric type information and provides more flexibility in metric processing.

## Components

1. **Metrics Producer** (`metrics-producer.yaml`)
   - A sample application that produces Prometheus metrics
   - Exposes metrics on port 8080
   - Deployed as a Kubernetes service for Prometheus to scrape

2. **Prometheus** (from `../prometheus-remote-write/prometheus-remote-write.yaml`)
   - Required to scrape and collect metrics from the producer
   - The OpenTelemetry Collector will scrape metrics from this Prometheus instance
   - Note: We'll use the same Prometheus configuration but without the remote_write section

3. **OpenTelemetry Collector** (`otel-collector.yaml`)
   - Uses the OpenTelemetry Collector Contrib image for advanced features
   - Configured to scrape Prometheus metrics
   - Transforms metrics and forwards them to Metoro
   - Includes all necessary Kubernetes resources (ConfigMap, Deployment, Service)

## Prerequisites

- Kubernetes cluster
- Metoro exporter installed in your cluster (namespace: `metoro`)
- `kubectl` configured to access your cluster

## Installation

1. Create the namespace and deploy the metrics producer:
```bash
kubectl apply -f metrics-producer.yaml
```

2. Deploy Prometheus (using a modified version without remote_write):
```bash
# Copy and modify the Prometheus configuration
cp ../prometheus-remote-write/prometheus-remote-write.yaml prometheus.yaml
# Remove the remote_write section from prometheus.yaml
kubectl apply -f prometheus.yaml
```

3. Deploy the OpenTelemetry Collector:
```bash
kubectl apply -f otel-collector.yaml
```

## Configuration Details

The collector configuration includes three main sections:

1. **Receivers**
```yaml
receivers:
  prometheus:
    config:
      scrape_configs:
        - job_name: "custom-metrics"
          scrape_interval: 15s
          static_configs:
            - targets:
                - "custom-metrics-producer.prometheus.svc.cluster.local:8080"
```

2. **Processors**
```yaml
processors:
  metricstransform:
    transforms:
      - include: '(.*)'
        match_type: regexp
        action: update
        new_name: 'otel_exporter_$1'
  batch: {}
```

3. **Exporters**
```yaml
exporters:
  otlphttp:
    endpoint: "http://metoro-exporter.metoro.svc.cluster.local/api/v1/custom/otel/metrics"
    tls:
      insecure: true
    encoding: json
```

## Advantages Over Remote Write

1. **Better Type Preservation**
   - Maintains counter, histogram, and summary types
   - Preserves metric metadata

2. **Metric Transformation**
   - Built-in support for metric name and label transformation
   - Can filter or modify metrics before sending

3. **Future Proofing**
   - OpenTelemetry is becoming the standard for observability
   - More features and processors available

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

## Customization

You can customize the collector configuration by modifying the ConfigMap:

1. **Add More Scrape Targets**
   - Add new jobs under `scrape_configs`
   - Configure different scrape intervals

2. **Modify Metric Names**
   - Adjust the `metricstransform` processor
   - Add custom prefixes or suffixes

3. **Add Processing Steps**
   - Include additional processors
   - Filter or aggregate metrics

## Additional Resources

- [Metoro Documentation](https://docs.metoro.io)
- [OpenTelemetry Collector Documentation](https://opentelemetry.io/docs/collector/)
- [OpenTelemetry Collector Contrib](https://github.com/open-telemetry/opentelemetry-collector-contrib) 