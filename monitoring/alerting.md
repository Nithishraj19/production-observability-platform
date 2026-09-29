# Alerting

Prometheus evaluates `prometheus/rules/alerts.yml` and sends alerts to Alertmanager. The default receiver is intentionally local and has no external integration or credential. Configure Slack, email, or incident tooling through a secret-managed overlay; group related alerts, set ownership and runbooks, and tune thresholds to observed service objectives. Test routing before relying on notifications.
