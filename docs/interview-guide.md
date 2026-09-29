# Interview guide

## Explain the data path

Prometheus pulls metrics from exporters and instrumented services, stores time series, and evaluates alert rules. Grafana queries Prometheus for exploration and dashboards. Alertmanager groups and routes firing alerts, reducing duplicate notifications and supporting silences during known maintenance.

## Discuss coverage

- Linux: node-exporter exposes host CPU, memory, filesystem, and network signals.
- Containers: cAdvisor reports resource usage and container lifecycle data.
- Kubernetes: node and pod discovery feeds targets; Kube State Metrics adds workload desired/current state and restart counters.
- Applications: instrumentation provides request counts, status codes, latency histograms, and health endpoints.
- AWS: CloudWatch integration is a documented option, not a deployed resource in this project.

## Operational tradeoffs

The included setup is a portfolio baseline with single-replica, ephemeral local monitoring. A production service needs durable storage, capacity planning, access control, TLS, secret handling, backup/restore, upgrade policy, and alert ownership. Exporter host mounts and cAdvisor privileges expand host access and should be reviewed against the cluster security model. Alert thresholds are examples and require service-specific tuning.

## Useful discussion prompts

1. How would you prevent high-cardinality labels from overwhelming Prometheus?
2. How would you choose retention and storage for the expected ingestion rate?
3. How do you make an alert actionable, deduplicated, and tied to a runbook?
4. How would you validate a latency SLO using histogram buckets?
5. Which signals belong in Prometheus and which remain in CloudWatch?
