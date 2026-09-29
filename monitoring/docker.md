# Docker monitoring

cAdvisor exposes container CPU, memory, and lifecycle metrics for Prometheus. The local Compose configuration is intended for a trusted development host. Container restart and health status availability depends on the runtime and exporter labels; validate metric names against the actual Docker host before writing environment-specific alerts.
