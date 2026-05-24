# Prometheus

## Overview

Prometheus is an open-source monitoring and alerting toolkit. It scrapes metrics from targets at configured intervals, evaluates rules, and can trigger alerts.

## Setup

### Run Prometheus with Docker
```bash
docker run -d \
  --name prometheus \
  -p 9090:9090 \
  prom/prometheus
```

### Run with custom config
```bash
docker run -d \
  --name prometheus \
  -p 9090:9090 \
  -v $(pwd)/prometheus.yml:/etc/prometheus/prometheus.yml \
  prom/prometheus
```

### Access Prometheus UI
```
http://localhost:9090
```

## Monitor CPU Usage

### Node Exporter — expose host metrics
```bash
docker run -d \
  --name node-exporter \
  -p 9100:9100 \
  --pid="host" \
  -v "/:/host:ro,rslave" \
  prom/node-exporter \
  --path.rootfs=/host
```

### Useful PromQL queries
```promql
# CPU usage per core
rate(node_cpu_seconds_total{mode!="idle"}[1m])

# Total CPU usage (%)
100 - (avg by(instance)(rate(node_cpu_seconds_total{mode="idle"}[1m])) * 100)

# Memory usage
node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes * 100

# Disk usage
100 - ((node_filesystem_avail_bytes * 100) / node_filesystem_size_bytes)
```

## Run Prometheus + Grafana with Docker Compose

```yaml
version: '3'
services:
  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml

  node-exporter:
    image: prom/node-exporter
    ports:
      - "9100:9100"

  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin
```

```bash
docker-compose up -d
# Grafana: http://localhost:3000 (admin/admin)
# Prometheus: http://localhost:9090
```

## sample prometheus.yml
```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node'
    static_configs:
      - targets: ['node-exporter:9100']
```

## References
1. https://devconnected.com/monitoring-linux-processes-using-prometheus-and-grafana/
2. https://prometheus.io/docs/introduction/overview/
3. https://prometheus.io/docs/prometheus/latest/querying/basics/
4. https://grafana.com/docs/grafana/latest/datasources/prometheus/
