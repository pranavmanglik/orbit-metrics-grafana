# Orbit Metrics Grafana

Orbit Metrics Grafana is a native Grafana App plugin that bundles dashboards for Orbit Core's runtime and HTTP metrics. It uses Grafana's built-in Prometheus data source; it does not add a metrics query API or require a backend service.

## Requirements

- Grafana 13.1 or later.
- A Prometheus data source that scrapes an Orbit application's `/metrics` endpoint.
- The Orbit application has `orbit-metrics-prometheus` installed and registered.

The Prometheus scrape endpoint requires the `orbit.metrics.read` role by default. Keep the endpoint behind an authenticated scrape identity or a trusted network boundary. The Grafana plugin stores no credentials and does not alter data-source security settings.

## Dashboards

- **Orbit Runtime Overview** — request and server-error rates, p95 request latency, service/plugin counts, failed tasks, container resolution failures, and service health.
- **Orbit HTTP Traffic** — response rates by method/status/outcome and p50, p95, and p99 latency.

Both dashboards include Prometheus, job, and instance selectors. Metric families are defined by Orbit Core and formatted by `orbit-metrics-prometheus`.

## Local development

```bash
npm ci
npm run typecheck
npm run lint
npm run test:ci
npm run build
```

To run Grafana with the plugin and its local provisioning configuration:

```bash
npm run server
```

The development container enables anonymous access and unsigned plugin loading for local iteration only. Do not use those settings in a production deployment. Configure a Prometheus data source separately; the plugin does not provision one or install Prometheus.

See [architecture](../docs/architecture/overview.md), [operations](../docs/operations/README.md), [security](../docs/security/overview.md), and [development](../docs/development/README.md).

## Status

Alpha review candidate; not yet published or signed. Hosted Grafana compatibility checks, signed artifact provenance, and deployment validation remain release gates.

Licensed under Apache-2.0.
