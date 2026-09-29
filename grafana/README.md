# Grafana

The provisioning directory registers Prometheus as the default datasource. The local stack uses the service name `prometheus`; the Kubernetes service is `prometheus` in namespace `observability`. Open Grafana and build or import dashboards appropriate to the monitored environment. No dashboards or credentials are provisioned in this baseline.
