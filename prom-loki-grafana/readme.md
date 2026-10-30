## 1. Observability Stack Setup
- Worked with Prometheus, Node Exporter, Grafana, Loki, and Grafana Alloy on an AWS EC2 Ubuntu instance using Docker Compose.
- Verified that Loki was receiving system logs and making them available in Grafana Explore.

## 2. Grafana Alloy Log Collection
- Updated `~/prometheus-lab/alloy/config.alloy` to collect logs from:
  - `/var/log/syslog` — system logs with the label `job="system"`.
  - `/var/log/auth.log` — authentication logs with the label `job="auth"`.
- Restarted Alloy and checked its container logs.
- Confirmed that Alloy was running and tailing both log files.

## 3. LogQL Queries
Practised using LogQL to filter and analyse logs in Grafana Explore.

- `{job="auth"}` — view authentication logs.
- `{job="system"} |= "TEST_ERROR"` — search for a specific test message.
- Used regular expressions to filter error messages and SSH authentication failures.
- Used `count_over_time()` and `sum()` to count matching log entries over a five-minute window.

## 4. Grafana Alerting
Worked on configuring alert rules for:

1. **SSH Login Failures** — detect log entries matching failed-password, invalid-user, or authentication-failure messages.
2. **System Log Errors** — detect error messages in system logs.
3. **Repeated Authentication Failures** — detect multiple matching authentication failures within five minutes.

Configured or reviewed alert thresholds, evaluation intervals, pending periods, folders, and notification contact points.

## 5. Alert Testing and Troubleshooting
- Generated test log messages using the Linux `logger` command.
- Checked system logs using `tail`.
- Troubleshot a Prometheus query parse error caused by selecting the wrong data source for a LogQL query.
- Identified that LogQL alert queries must use Loki as the data source, not Prometheus.

## Key Learnings
- Difference between system logs and authentication logs.
- How Alloy collects and forwards logs to Loki.
- How to query logs using LogQL.
- How Grafana evaluates alert conditions and pending periods.
- How to test log collection and troubleshoot alert queries.

## Next Steps
- Verify that all three alert rules are saved and evaluate correctly.
- Confirm that alert notifications reach the configured contact point.
- Test repeated-authentication alert logic safely.
- Review alert states in Grafana and document the results.
## Screenshots
![]()
![]()
![]()