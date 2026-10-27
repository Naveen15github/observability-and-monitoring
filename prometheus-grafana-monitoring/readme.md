# AWS EC2 Monitoring and Alerting with Prometheus, Grafana and Slack

This hands-on demonstrates infrastructure monitoring on an AWS EC2 instance using Docker Compose, Prometheus, Node Exporter, and Grafana. It covers metric collection, dashboard visualization, infrastructure alerting, and Slack notification integration.

## Technologies Used

- AWS EC2
- Linux
- Docker and Docker Compose
- Prometheus
- Node Exporter
- Grafana
- PromQL
- Slack alert notifications

## Project Structure

```text
prometheus-lab/
├── README.md
├── compose.yaml
└── prometheus.yml
```

## 1. Monitoring Stack Deployment

Configured the monitoring stack using `compose.yaml`.

- **Prometheus:** Collects and stores metrics on port 9090.
- **Node Exporter:** Exposes Linux host metrics for Prometheus scraping.
- **Grafana:** Provides dashboards and alerting on port 3000.
- **Docker volumes:** Preserve Prometheus and Grafana data across container restarts.

Started the services using Docker Compose and observed the containers reaching their running or started states.

### Commands

```bash
sudo docker compose config
sudo docker compose up -d
sudo docker compose ps
```

## 2. Prometheus Configuration

Configured `prometheus.yml` with a global scrape interval of 15 seconds.

| Job | Target | Purpose |
|---|---|---|
| prometheus | `localhost:9090` | Collect Prometheus metrics |
| node-exporter | `node-exporter:9100` | Collect host system metrics |

## 3. Grafana Infrastructure Dashboard

Reviewed the EC2 Prometheus Monitoring dashboard with the Prometheus data source.

The dashboard included the following panels:

- CPU Usage (%)
- Memory Usage (%)
- Disk Usage (%)
- Target Health
- Network Traffic (bytes/sec)
- System Load Average
- Disk I/O (bytes/sec)
- Running / Blocked Processes

The dashboard provided a visual overview of system resource utilization and monitoring-target health over time.

## 4. Infrastructure Alert Rules

Created or reviewed Grafana-managed alert rules under the Infrastructure Alerts folder and Infrastructure Evaluation group.

The rules shown in the dashboard included:

- **High CPU Usage:** Detects CPU utilization above the configured threshold. The Slack alert description indicated usage above 80% for 5 minutes.
- **High Memory Usage:** Detects high memory utilization.
- **High Disk Usage:** Detects high disk utilization.
- **Node Exporter Down:** Detects when the Node Exporter target becomes unavailable.

Reviewed alert states, including firing, normal, and resolved states.

## 5. Slack Notification Integration

Verified Grafana alert notifications in the Slack `#all-infra-alerts` channel through the Grafana Alerts app.

Observed notifications included:

- TestAlert Grafana
- High CPU Usage
- High Memory Usage
- High Disk Usage
- Node Exporter Down

Observed both firing and resolved notification messages, demonstrating alert-state notifications reaching Slack.

## 6. Validation and Troubleshooting

- Validated the Docker Compose configuration.
- Started the monitoring services on EC2.
- Reviewed container startup status.
- Opened the Grafana dashboard and inspected resource metrics.
- Reviewed Grafana alert rules and their states.
- Verified Slack alert delivery.

An initial Docker socket permission error occurred when running Compose without sufficient privileges. Retrying with `sudo docker compose up -d` allowed the deployment command to proceed.

## Screenshots
(https://github.com/Naveen15github/observability-and-monitoring/blob/752cd5f3872c6401333f8ec260e2366ea70111aa/prometheus-grafana-monitoring/images/Screenshot%20(699).png)

(https://github.com/Naveen15github/observability-and-monitoring/blob/752cd5f3872c6401333f8ec260e2366ea70111aa/prometheus-grafana-monitoring/images/Screenshot%20(701).png)

(https://github.com/Naveen15github/observability-and-monitoring/blob/752cd5f3872c6401333f8ec260e2366ea70111aa/prometheus-grafana-monitoring/images/Screenshot%20(702).png)

(https://github.com/Naveen15github/observability-and-monitoring/blob/752cd5f3872c6401333f8ec260e2366ea70111aa/prometheus-grafana-monitoring/images/Screenshot%20(703).png)

(https://github.com/Naveen15github/observability-and-monitoring/blob/752cd5f3872c6401333f8ec260e2366ea70111aa/prometheus-grafana-monitoring/images/Screenshot%20(704).png)

(https://github.com/Naveen15github/observability-and-monitoring/blob/752cd5f3872c6401333f8ec260e2366ea70111aa/prometheus-grafana-monitoring/images/Screenshot%20(705).png)

(https://github.com/Naveen15github/observability-and-monitoring/blob/752cd5f3872c6401333f8ec260e2366ea70111aa/prometheus-grafana-monitoring/images/Screenshot%20(706).png)

(https://github.com/Naveen15github/observability-and-monitoring/blob/752cd5f3872c6401333f8ec260e2366ea70111aa/prometheus-grafana-monitoring/images/Screenshot%20(707).png)

## Key Learnings

- Deploying a monitoring stack using Docker Compose.
- Configuring Prometheus scrape jobs and targets.
- Collecting Linux system metrics with Node Exporter.
- Building infrastructure dashboards in Grafana.
- Defining resource-utilization and availability alerts.
- Integrating Grafana alert notifications with Slack.
- Observing firing and resolved alert states.
- Troubleshooting Docker permissions on an EC2 instance.

## Summary

Today's hands-on covered the deployment of Prometheus, Node Exporter, and Grafana on AWS EC2, followed by dashboard inspection, infrastructure alert-rule configuration, and Slack notification verification. The work provided practical exposure to infrastructure monitoring, alerting, and operational visibility.


