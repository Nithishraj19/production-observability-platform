# Production Observability Platform

A portfolio reference project for monitoring Linux hosts, Docker containers, Kubernetes resources, and application health with Prometheus, Grafana, and Alertmanager. It includes a local Docker Compose path and Kubernetes manifests for a cluster you already operate.

## Architecture

Metrics flow from hosts and workloads through exporters and application instrumentation to Prometheus. Grafana queries Prometheus for dashboards; Prometheus evaluates alert rules and sends firing alerts to Alertmanager, which routes notifications to configured receivers.

See [architecture/architecture.md](architecture/architecture.md) for boundaries and the [monitoring guides](monitoring/) for coverage. AWS services are an integration reference only; this repository does not provision AWS infrastructure.

## Local Docker path

Requirements: Docker Engine and the Docker Compose plugin. From the repository root:

```sh
docker compose -f docker/docker-compose.yml up -d
```

Open Prometheus at <http://localhost:9090>, Grafana at <http://localhost:3000>, and Alertmanager at <http://localhost:9093>. Grafana's upstream image uses its documented initial `admin` / `admin` login when no credentials are provided; set a unique password at first login and keep Grafana bound to localhost. Prometheus scrapes itself, node-exporter, cAdvisor, and the demo application. Stop with:

```sh
docker compose -f docker/docker-compose.yml down
```

The Compose file mounts the Docker socket read-only for cAdvisor. A read-only mount does not remove the socket's inherent host access implications; use this only on a trusted local machine.

## Kubernetes path

The manifests under `kubernetes/` install a single-replica Prometheus and Grafana in the `observability` namespace plus node-exporter and cAdvisor DaemonSets. Apply them to a disposable or existing cluster with:

```sh
kubectl apply -f kubernetes/prometheus/namespace.yaml
kubectl apply -R -f kubernetes/
```

Access services with `kubectl port-forward`. These manifests are a learning baseline, not a hardened multi-tenant deployment: review RBAC, storage, resource sizing, TLS, credentials, and network policy before production use. Kube State Metrics is documented as an integration and is not included as a deployment here.

## AWS reference

`aws/cloudwatch.md` describes possible integration points for EC2, EKS, load balancers, Auto Scaling, RDS, and CloudWatch. No Terraform, AWS credentials, or deployment automation is included. Do not run cloud provisioning from this repository.

## Repository map

- `prometheus/`: scrape configuration and alert rules
- `grafana/`: datasource provisioning
- `alertmanager/`: routing and local receiver configuration
- `docker/`: local demo stack
- `kubernetes/`: cluster manifests
- `monitoring/`: monitoring scope by signal source
- `docs/interview-guide.md`: architecture and operational discussion prompts

Configuration validation can be done with `promtool check config prometheus/prometheus.yml`, `promtool check rules prometheus/rules/alerts.yml`, `docker compose -f docker/docker-compose.yml config`, and `kubectl apply --dry-run=client -f kubernetes/`.
