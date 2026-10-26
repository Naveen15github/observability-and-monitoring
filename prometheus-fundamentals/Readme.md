# Prometheus Fundamentals — Monitoring & Observability Lab

A hands-on infrastructure monitoring project using **Prometheus, Grafana, Node Exporter, Docker, Docker Compose, and AWS EC2**.

This project demonstrates how Linux system metrics can be collected using Prometheus, exposed through Node Exporter, queried using PromQL, and visualized in Grafana dashboards.

## Project Overview

The monitoring environment runs on an Ubuntu EC2 instance in AWS. Docker Compose manages the monitoring services, Prometheus collects metrics, Node Exporter exposes Linux host metrics, and Grafana provides dashboards for monitoring system performance.

## Architecture

```text
                     AWS Cloud
                         |
                      AWS EC2
                    Ubuntu Linux
                         |
                   Docker Compose
                         |
           +-------------+-------------+
           |             |             |
           v             v             v
      Prometheus    Node Exporter    Grafana
        :9090          :9100          :3000
           |             |
           |             v
           |       Linux Host Metrics
           |       - CPU
           |       - Memory
           |       - Disk
           |       - Network
           |       - System Load
           |       - Processes
           |
           v
      Metrics Collection
           |
           v
      Grafana Data Source
           |
           v
      Monitoring Dashboard
```

## Technology Stack

| Technology | Purpose |
|---|---|
| AWS EC2 | Hosts the monitoring environment |
| Ubuntu Linux | Operating system |
| AWS Systems Manager (SSM) | Access to the EC2 instance |
| Docker | Runs the monitoring services in containers |
| Docker Compose | Manages the multi-container environment |
| Prometheus | Collects and stores time-series metrics |
| Node Exporter | Exposes Linux host metrics |
| Grafana | Visualizes metrics using dashboards |
| PromQL | Queries Prometheus metrics |

## Components Implemented

### Prometheus

Prometheus collects metrics from configured monitoring targets at regular intervals.

Implemented:
- Configured a 15-second scrape interval.
- Configured self-monitoring for Prometheus.
- Configured metric collection from Node Exporter.
- Verified the health of both configured targets.
- Practiced PromQL queries.

URL: `http://<EC2-PUBLIC-IP>:9090`

### Node Exporter

Node Exporter exposes Linux system metrics for Prometheus to scrape.

Metrics explored:
- CPU utilization
- Memory usage
- Disk usage
- Network traffic
- System load average
- Disk I/O
- Running and blocked processes

### Grafana

Grafana is connected to Prometheus as its data source.

Data source URL:

`http://prometheus:9090`

An eight-panel dashboard was created to visualize infrastructure metrics and target health.

URL: `http://<EC2-PUBLIC-IP>:3000`

### Docker Compose

Docker Compose is used to deploy and manage Prometheus, Node Exporter, and Grafana through a single YAML configuration file.

## Grafana Dashboard

The current dashboard contains eight panels.

| No. | Panel | Purpose |
|---:|---|---|
| 1 | CPU Usage (%) | Displays CPU utilization |
| 2 | Memory Usage (%) | Displays memory consumption |
| 3 | Disk Usage (%) | Displays filesystem utilization |
| 4 | Target Health | Displays Prometheus target availability |
| 5 | Network Traffic | Displays received and transmitted traffic rates |
| 6 | System Load Average | Displays 1-, 5-, and 15-minute load averages |
| 7 | Disk I/O | Displays disk read and write activity |
| 8 | Running/Blocked Processes | Displays process state metrics |

## Screenshots

## Screenshots

### 1. Grafana Monitoring Dashboard
![Grafana Monitoring Dashboard](./images/Screenshot%20(688).png)

### 2. AWS Systems Manager Setup
![AWS Systems Manager Setup](./images/Screenshot%20(689).png)

### 3. Prometheus Container Logs
![Prometheus Container Logs](./images/Screenshot%20(690).png)

### 4. Prometheus Targets
![Prometheus Targets](./images/Screenshot%20(691).png)

### 5. Docker Containers and Configuration
![Docker Containers and Configuration](./images/Screenshot%20(692).png)

### 6. Healthy Prometheus Targets
![Healthy Prometheus Targets](./images/Screenshot%20(693).png)

### 7. PromQL Metrics Query
![PromQL Metrics Query](./images/Screenshot%20(694).png)

### 8. PromQL Metrics Graph
![PromQL Metrics Graph](./images/Screenshot%20(695).png)

### 9. Additional Monitoring Screenshot
![Additional Monitoring Screenshot](./images/Screenshot%20(698).png)
## Project Structure

```text
prometheus-fundamentals/
├── README.md
├── compose.yaml
├── prometheus.yml
├── .gitignore
└── images/
    ├── Screenshot (688).png
    ├── Screenshot (689).png
    ├── Screenshot (690).png
    ├── Screenshot (691).png
    ├── Screenshot (692).png
    ├── Screenshot (693).png
    ├── Screenshot (694).png
    ├── Screenshot (695).png
    └── Screenshot (698).png
```

## Configuration Files

### Docker Compose — `compose.yaml`

```yaml
services:
  prometheus:
    image: prom/prometheus
    container_name: prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml:ro
    command:
      - "--config.file=/etc/prometheus/prometheus.yml"
    restart: unless-stopped

  node-exporter:
    image: prom/node-exporter
    container_name: node-exporter
    command:
      - "--path.procfs=/host/proc"
      - "--path.sysfs=/host/sys"
      - "--path.rootfs=/rootfs"
    volumes:
      - "/proc:/host/proc:ro"
      - "/sys:/host/sys:ro"
      - "/:/rootfs:ro,rslave"
    expose:
      - "9100"
    restart: unless-stopped

  grafana:
    image: grafana/grafana
    container_name: grafana
    ports:
      - "3000:3000"
    restart: unless-stopped
```

### Prometheus — `prometheus.yml`

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]

  - job_name: "node-exporter"
    static_configs:
      - targets: ["node-exporter:9100"]
```

Prometheus scrapes the configured targets every 15 seconds. Docker Compose networking allows Prometheus to communicate with Node Exporter through the service hostname `node-exporter`.

## PromQL Queries Practiced

### Target Health

```promql
up
```

Returns `1` when a target scrape succeeds and `0` when it fails.

### CPU Usage (%)

```promql
100 * (1 - avg by (instance) (
  rate(node_cpu_seconds_total{mode="idle"}[5m])
))
```

Calculates CPU utilization based on the proportion of CPU time not spent idle.

### Memory Usage (%)

```promql
100 * (
  1 - node_memory_MemAvailable_bytes
  / node_memory_MemTotal_bytes
)
```

Estimates memory utilization using available and total memory.

### Disk Usage (%)

```promql
100 * (
  1 -
  node_filesystem_avail_bytes{
    mountpoint="/",
    fstype!~"tmpfs|overlay"
  }
  /
  node_filesystem_size_bytes{
    mountpoint="/",
    fstype!~"tmpfs|overlay"
  }
)
```

Estimates root filesystem utilization. Results depend on the filesystem metrics and labels available on the host.

### Network Receive Rate

```promql
rate(node_network_receive_bytes_total[5m])
```

Measures received network bytes per second.

### Network Transmit Rate

```promql
rate(node_network_transmit_bytes_total[5m])
```

Measures transmitted network bytes per second.

### System Load Average

```promql
node_load1
```

```promql
node_load5
```

```promql
node_load15
```

These metrics represent the 1-, 5-, and 15-minute system load averages.

### Disk Read Rate

```promql
rate(node_disk_read_bytes_total[5m])
```

### Disk Write Rate

```promql
rate(node_disk_written_bytes_total[5m])
```

These queries calculate disk read and write throughput.

## Deployment and Verification

Start the services:

```bash
docker compose up -d
```

Check container status:

```bash
docker compose ps
```

Inspect service logs:

```bash
docker compose logs --tail=50
```

Check Prometheus targets:

`http://<EC2-PUBLIC-IP>:9090/targets`

The Prometheus and Node Exporter targets were verified as healthy during the lab setup.

## Key Learnings

- Infrastructure monitoring fundamentals
- Deploying monitoring services using Docker Compose
- Configuring Prometheus scrape targets
- Collecting Linux metrics with Node Exporter
- Connecting Grafana to Prometheus
- Creating a multi-panel monitoring dashboard
- Querying time-series metrics using PromQL
- Checking monitoring target health
- Inspecting container status and logs
- Accessing an AWS EC2 instance using Systems Manager

## Security Considerations

- Restrict access to Grafana and Prometheus to trusted IP addresses or private networks.
- Do not expose Node Exporter's port `9100` publicly.
- Avoid exposing monitoring interfaces to the entire internet.
- Do not commit AWS credentials, private keys, passwords, or tokens.
- Consider persistent storage and backups before using the environment for long-term monitoring.

## References

- [Prometheus Documentation](https://prometheus.io/docs/)
- [PromQL Documentation](https://prometheus.io/docs/prometheus/latest/querying/basics/)
- [Node Exporter](https://github.com/prometheus/node_exporter)
- [Grafana Documentation](https://grafana.com/docs/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [AWS Systems Manager Documentation](https://docs.aws.amazon.com/systems-manager/)

## Author

**Naveen G.**

Cloud, DevOps and AI Enthusiast

- GitHub: [Naveen15github](https://github.com/Naveen15github)
- LinkedIn: [Naveen G.](https://www.linkedin.com/in/naveen-g-41a041253/)

---

*Hands-on learning project focused on Prometheus, Grafana, Linux infrastructure metrics, and monitoring fundamentals.*
