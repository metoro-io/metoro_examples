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

2. Deploy Prometheus with Remote Write configuration:
```bash
kubectl apply -f prometheus-remote-write.yaml
```

## Verification

1. Check that all pods are running:
```bash
kubectl get pods -n prometheus
```

2. Verify Prometheus is scraping metrics:
```bash
kubectl port-forward -n prometheus svc/prometheus 9090:9090
```
Then visit `http://localhost:9090/targets` in your browser

3. Check your Metoro dashboard for the incoming metrics

## Configuration Details

The Prometheus configuration includes:

```yaml
remote_write:
  - url: "http://metoro-exporter.metoro.svc.cluster.local/api/v1/send/prometheus/remote_write"
```

This directs Prometheus to send all scraped metrics to the Metoro exporter using Remote Write.

## Limitations

- Some metric types (counters, histograms, summaries) may be converted to gauges due to limitations in the Remote Write protocol
- No built-in support for metric transformation or filtering
- Limited metadata preservation

## Troubleshooting

1. Check Prometheus logs:
```bash
kubectl logs -n prometheus deployment/prometheus
```

2. Verify network connectivity:
```bash
kubectl exec -n prometheus deploy/prometheus -- wget -q -O- http://metoro-exporter.metoro.svc.cluster.local/health
```

3. Common issues:
   - Ensure the Metoro exporter service is running and accessible
   - Check that the Remote Write URL is correct
   - Verify that metrics are being scraped from the producer

## Additional Resources

- [Metoro Documentation](https://docs.metoro.io)
- [Prometheus Remote Write Documentation](https://prometheus.io/docs/operating/integrations/#remote-endpoints-and-storage) 