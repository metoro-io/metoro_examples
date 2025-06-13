# Nvidia Dashboard Setup

This document will walk you through the installation process to install the [nvidia-dcgm-exporter](https://github.com/NVIDIA/dcgm-exporter) into your cluster and to set up a dashboard to visualize the metrics it produces.

First install the `nvidia-dcgm-exporter` into your cluster by running the following command:

```bash
helm upgrade \
    --install \
    dcgm-exporter \
    gpu-helm-charts/dcgm-exporter \
    --set serviceMonitor.enabled="false" \
    --set podLabels."app"="nvidia-dcgm-exporter"
```

Then install the otel collector with the following command:
```bash
k apply -f ./
```

Check the logs of the otel collector to ensure that it is scraping correctly, you should see something like this:
```bash
kubectl logs -n default deployment/nvidia-otel-collector
```
```text
2025-06-13T09:37:39.766Z	debug	otlphttpexporter@v0.118.0/otlp.go:177	Preparing to make HTTP request	{"kind": "exporter", "data_type": "metrics", "name": "otlphttp", "url": "http://metoro-exporter.metoro.svc.cluster.local/api/v1/custom/otel/metrics"}
2025-06-13T09:37:39.765Z	info	Metrics	{"kind": "exporter", "data_type": "metrics", "name": "debug", "resource metrics": 1, "metrics": 29, "data points": 29}
```

You can then create the dashboard in Metoro by going to the dashboard creation page and adding the template for the nvidia dashboard.
