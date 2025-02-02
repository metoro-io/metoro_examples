# Vector Logs Example

This example demonstrates how to send logs to Metoro using Vector and the OpenTelemetry Collector.

## Overview

The setup consists of:
1. Vector deployment that generates sample syslog messages
2. OpenTelemetry Collector that receives logs via syslog and forwards them to Metoro
3. Required Kubernetes resources (services, RBAC, etc.)

## Prerequisites

- A Kubernetes cluster
- Metoro installed in your cluster
- `kubectl` configured to interact with your cluster

## Installation

1. Create the vector namespace:
```bash
kubectl create namespace vector
```

2. Apply the configuration:
```bash
kubectl apply -f vector.yaml
```

## Verification

1. Check that Vector is running:
```bash
kubectl get pods -n vector -l app=vector
```

2. Check that the OpenTelemetry Collector is running:
```bash
kubectl get pods -n vector -l app=otel-collector
```

3. View the OpenTelemetry Collector logs to see the incoming data:
```bash
kubectl logs -n vector -l app=otel-collector
```

## Configuration Details

The example uses:
- Vector's demo_logs source to generate sample syslog messages
- Vector's socket sink to forward logs to the OpenTelemetry Collector
- OpenTelemetry Collector's syslog receiver and OTLP HTTP exporter
- Batch processing for efficient log forwarding

For more details, see the [Vector documentation](https://vector.dev/docs/) in the Metoro docs. 