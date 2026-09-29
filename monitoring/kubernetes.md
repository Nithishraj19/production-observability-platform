# Kubernetes monitoring

Use node-exporter and cAdvisor DaemonSets for node and container telemetry. Kube State Metrics provides object state such as pods, deployments, resource requests/limits, and restart counters; the cluster configuration includes a scrape target for a Kube State Metrics service assumed in `kube-system`, but does not install it. Prometheus uses Kubernetes service discovery and needs the included read-only discovery RBAC. Review host access, privileges, RBAC, storage, and resource requests for your cluster.
