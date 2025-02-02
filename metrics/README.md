# Metoro Examples

This repository contains examples of how to send metrics to Metoro using different methods. Each example demonstrates a different approach to integrating with Metoro's observability platform.

## Examples

### 1. Prometheus Remote Write

Located in `prometheus-remote-write/`, this example shows how to configure Prometheus to send metrics directly to Metoro using Prometheus Remote Write protocol.

### 2. OpenTelemetry Collector (Recommended)

Located in `otel-collector/`, this example demonstrates how to use the OpenTelemetry Collector to scrape Prometheus metrics and forward them to Metoro. This is the recommended approach as it better preserves metric type information.


## Why OpenTelemetry Collector?

While both methods work, we recommend using the OpenTelemetry Collector approach because:

1. Better type preservation - OpenTelemetry has a more sophisticated type system than Prometheus Remote Write
2. More flexibility - The collector can be configured to transform and process metrics before sending them
3. Future-proof - OpenTelemetry is becoming the standard for observability data collection

## Prerequisites

- A Kubernetes cluster
- Metoro exporter installed in your cluster
- kubectl configured to access your cluster