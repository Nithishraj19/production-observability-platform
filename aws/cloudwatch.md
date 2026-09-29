# AWS and CloudWatch integration reference

This document describes a possible observability integration for AWS hosted workloads. It is not a deployment guide and this repository contains no AWS provisioning configuration.

| Service | Signals and integration idea |
| --- | --- |
| EC2 | Install node-exporter for host metrics; use CloudWatch Agent where CloudWatch-native metrics/logs are required. |
| EKS | Scrape Prometheus endpoints and Kube State Metrics; protect cluster API access with least-privilege RBAC. |
| ALB / NLB | Consume CloudWatch request, target health, and throughput metrics through a CloudWatch exporter or remote integration. |
| Auto Scaling | Correlate instance lifecycle and capacity metrics with host and application signals. |
| RDS | Use CloudWatch service metrics; avoid exposing database credentials in scrape configuration. |
| CloudWatch | Select a managed or self-hosted bridge according to retention, access, and cost requirements. |

Keep workload and monitoring traffic inside approved network paths, restrict security groups to required sources, and store notification credentials in a secret manager. This portfolio project does not create VPCs, EC2, EKS, ALB/NLB, Auto Scaling groups, RDS, or CloudWatch resources. No AWS deployment has been performed.
