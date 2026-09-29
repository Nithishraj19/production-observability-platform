# Application monitoring

Instrument applications with a Prometheus client and expose `/metrics`. Track request totals by status and a duration histogram to derive error rate and p95 latency. Provide a separate `/health` endpoint for service health checks. The alert rules expect `http_requests_total` and `http_request_duration_seconds_bucket`; applications that use different metric names need matching rules.
