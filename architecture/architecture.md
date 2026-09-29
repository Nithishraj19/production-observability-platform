# Architecture

![Production observability platform architecture](architecture.png)

## Signal flow

```mermaid
flowchart LR
  subgraph sources[Monitored sources]
    linux[Linux hosts\nnode-exporter]
    containers[Docker hosts\ncAdvisor]
    kube[Kubernetes\nKube State Metrics + node-exporter]
    app[Instrumented applications\n/metrics and /health]
    aws[AWS services\nCloudWatch integration reference]
  end
  prom[Prometheus\nscrape, store, evaluate rules]
  graf[Grafana\ndashboards and exploration]
  am[Alertmanager\ngroup, route, silence]
  notify[Slack / email / incident receiver]
  linux -->|scrape| prom
  containers -->|scrape| prom
  kube -->|scrape| prom
  app -->|scrape| prom
  aws -. optional exporter or remote integration .-> prom
  prom -->|PromQL| graf
  prom -->|firing alerts| am
  am --> notify
```

## Boundaries and deployment choices

The local Compose stack is a single-host demonstration. Prometheus scrapes local exporters and the demo application; Grafana and Alertmanager consume that Prometheus instance. In Kubernetes, Prometheus and Grafana run in the `observability` namespace, while node-exporter and cAdvisor run as DaemonSets. Application `/metrics` endpoints and Kube State Metrics must be added to scrape configuration for a target cluster.

AWS is an architecture reference: EC2/EKS workloads can expose Prometheus metrics, and CloudWatch metrics can be bridged using an appropriate exporter or remote integration. This repository does not create VPCs, load balancers, clusters, databases, or other cloud resources.
