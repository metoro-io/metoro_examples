# Metoro Examples

This repository contains examples of how to send metrics to Metoro using different methods. Each example demonstrates a different approach to integrating with Metoro's observability platform.

## Examples

### 1. Prometheus Remote Write

Located in `prometheus-remote-write/`, this example shows how to configure Prometheus to send metrics directly to Metoro using Prometheus Remote Write protocol.

Key files:
- `metrics-producer.yaml`: A sample application that produces metrics
- `prometheus-remote-write.yaml`: Prometheus configuration with remote write enabled

To deploy:
```bash
kubectl apply -f prometheus-remote-write/metrics-producer.yaml
kubectl apply -f prometheus-remote-write/prometheus-remote-write.yaml
```

### 2. OpenTelemetry Collector (Recommended)

Located in `otel-collector/`, this example demonstrates how to use the OpenTelemetry Collector to scrape Prometheus metrics and forward them to Metoro. This is the recommended approach as it better preserves metric type information.

Key files:
- `metrics-producer.yaml`: A sample application that produces metrics
- `otel-collector.yaml`: OpenTelemetry Collector configuration that scrapes Prometheus and forwards to Metoro

To deploy:
```bash
kubectl apply -f otel-collector/metrics-producer.yaml
kubectl apply -f otel-collector/otel-collector.yaml
```

## Why OpenTelemetry Collector?

While both methods work, we recommend using the OpenTelemetry Collector approach because:

1. Better type preservation - OpenTelemetry has a more sophisticated type system than Prometheus Remote Write
2. More flexibility - The collector can be configured to transform and process metrics before sending them
3. Future-proof - OpenTelemetry is becoming the standard for observability data collection

## Prerequisites

- A Kubernetes cluster
- Metoro exporter installed in your cluster
- kubectl configured to access your cluster

## Getting Started

1. Clone this repository:
```bash
git clone https://github.com/metoro-k8s/metoro_examples.git
cd metoro_examples
```

2. Choose your preferred method (OpenTelemetry Collector or Prometheus Remote Write)

3. Deploy the example:
```bash
# For OpenTelemetry Collector (recommended)
kubectl apply -f otel-collector/

# OR for Prometheus Remote Write
kubectl apply -f prometheus-remote-write/
```

4. Verify that metrics are being received in your Metoro dashboard

## Support

If you encounter any issues or have questions, please:
1. Check our [documentation](https://docs.metoro.io)
2. Open an issue in this repository
3. Contact our support team

## License

MIT License - See LICENSE file for details 