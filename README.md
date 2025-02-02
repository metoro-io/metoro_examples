# Metoro Examples

This repository contains examples and guides for integrating with Metoro's observability platform. Each directory contains self-contained examples for different aspects of the platform.

## Available Examples

### [Metrics Integration](./metrics/)
Examples for sending metrics to Metoro:
- Prometheus Remote Write integration
- OpenTelemetry Collector setup (recommended)

## Repository Structure

```
metoro_examples/
├── metrics/                          # Metrics integration examples
│   ├── otel-collector/               # OpenTelemetry Collector example
│   └── prometheus-remote-write/      # Prometheus Remote Write example
└── ... (more examples coming soon)
```

## Prerequisites

- Kubernetes cluster
- Metoro exporter installed in your cluster
- `kubectl` configured to access your cluster

## Getting Started

1. Clone this repository:
```bash
git clone https://github.com/metoro-k8s/metoro_examples.git
cd metoro_examples
```

2. Navigate to the example you want to try:
```bash
cd metrics/otel-collector  # or any other example directory
```

3. Follow the README in that directory for specific instructions

## Contributing

We welcome contributions! If you have an example you'd like to add:

1. Create a new directory with a descriptive name
2. Include a detailed README.md
3. Ensure all examples are self-contained and well-documented
4. Submit a pull request

## Support

If you encounter any issues or have questions:
1. Check our [documentation](https://docs.metoro.io)
2. Open an issue in this repository
3. Contact our support team

## License

MIT License - See LICENSE file for details 
