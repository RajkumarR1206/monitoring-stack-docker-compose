---

# Monitoring Stack with Prometheus, Node Exporter, Grafana & Alertmanager

A complete system monitoring and alerting setup using Docker Compose.

---

## Overview

This project deploys a full observability stack using Docker Compose. It monitors host machine metrics, visualizes them in Grafana, and triggers email alerts using Alertmanager.

### Components:

* **Prometheus** – Metrics collection
* **Node Exporter** – Host system metrics exporter
* **Grafana** – Dashboards and visualization
* **Alertmanager** – Email alert routing
* **Custom Alerts** – High CPU alerting

The entire stack is fully containerized and runs on **Linux / WSL / Docker Desktop**.

---

## Features

* Complete monitoring setup using `docker-compose`
* Host-level CPU, Memory, Disk monitoring via Node Exporter
* Custom Grafana dashboards included (CPU, Memory, Disk)
* Alerting for high CPU usage using Prometheus + Alertmanager
* Email notifications through Gmail SMTP
* Prometheus rules & Alertmanager configuration included
* Easily extendable with more exporters

---

## Project Structure

```
monitoring-stack/
├── docker-compose.yml
├── README.md
├── alertmanager.yml
├── alerts/
│   └── alert-rules.yml
├── dashboards/
│   ├── CPU Usage (%).json
│   ├── Memory Usage (%).json
│   └── Disk usage (%).json
├── prometheus/
│   └── prometheus.yml
└── grafana/
```

---

##  Sample Output:

* Monitoring using Docker compose:
<img width="1349" height="165" alt="docker-compose-new" src="https://github.com/user-attachments/assets/ac8ebf29-dd08-4dc1-8af6-94c8691cb391" />

<img width="1358" height="151" alt="containers" src="https://github.com/user-attachments/assets/bfadf240-49e7-40e8-88a8-e334a77b7379" />


* Node Exporter Metrics:
<img width="1356" height="536" alt="Node-Exporter-Full-Dashboard" src="https://github.com/user-attachments/assets/bec28b4d-450d-4629-83b9-326b9fd4f31c" />


* Grafana dashboards for CPU, Memory & Disk:
<img width="1341" height="485" alt="Dashboards" src="https://github.com/user-attachments/assets/f13d5f25-274c-4574-874f-7b2d8e59db48" />

CPU usage:
<img width="1360" height="584" alt="CPU-Usage" src="https://github.com/user-attachments/assets/9c6ca554-176f-4e60-a248-5badfc2f5a58" />

Memory Usage:
<img width="1360" height="597" alt="Memory-usage" src="https://github.com/user-attachments/assets/41b1ae0c-1a25-41bd-a7d0-ac3881228be0" />

Disk Usage:
<img width="1347" height="584" alt="Disk-usage" src="https://github.com/user-attachments/assets/aacfc539-d1f3-4c18-960b-31f8003994ab" />

Firing Alert:
<img width="1352" height="524" alt="Prometheus-alert-Firing" src="https://github.com/user-attachments/assets/c0da8e69-f1e2-4b14-986d-bf109322308f" />

Alert Manager:
<img width="1105" height="446" alt="Alertmanager" src="https://github.com/user-attachments/assets/ef4e2b92-11e6-4ff1-80e9-ad0e52f5166e" />

Received alert mail on Configured Gmail:
<img width="1309" height="547" alt="alertmail" src="https://github.com/user-attachments/assets/73d5744f-16a4-4d60-8d0e-887493810b08" />


---

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/<your-username>/monitoring-stack.git
cd monitoring-stack
```

### 2. Start All Services

```bash
docker compose up -d
```

### 3. Verify Running Containers

```bash
docker ps
```

---

## Access the Services

| Service       | URL                                                            |
| ------------- | -------------------------------------------------------------- |
| Prometheus    | [http://localhost:9090](http://localhost:9090)                 |
| Grafana       | [http://localhost:3000](http://localhost:3000)                 |
| Alertmanager  | [http://localhost:9093](http://localhost:9093)                 |
| Node Exporter | [http://localhost:9100/metrics](http://localhost:9100/metrics) |

---

## Prometheus Configuration (`prometheus/prometheus.yml`)

Prometheus scrapes Node Exporter every 5 seconds and loads alert rules:

```yaml
global:
  scrape_interval: 5s

scrape_configs:
  - job_name: "node"
    static_configs:
      - targets: ["localhost:9100"]

alerting:
  alertmanagers:
    - static_configs:
        - targets: ["alertmanager:9093"]

rule_files:
  - /etc/prometheus/alert-rules.yml
```

---

## Alerting Rules (`alerts/alert-rules.yml`)

The stack includes a high CPU alert using Pressure Stall Information (PSI):

```yaml
groups:
  - name: node_alerts
    rules:
      - alert: HighCPUUsage
        expr: rate(node_pressure_cpu_waiting_seconds_total[30s]) > 0.12
        for: 10s
        labels:
          severity: warning
        annotations:
          summary: "High CPU Usage"
          description: "CPU is overloaded for more than 10 seconds."
```

---

## Email Alerting (Alertmanager)

Alertmanager is configured to send email notifications using Gmail SMTP.

`alertmanager.yml`:

```yaml
global:
  smtp_smarthost: 'smtp.gmail.com:587'
  smtp_from: 'your-email@gmail.com'
  smtp_auth_username: 'your-email@gmail.com'
  smtp_auth_password: 'your-app-password'
  smtp_require_tls: true

route:
  receiver: 'gmail-notifications'

receivers:
  - name: 'gmail-notifications'
    email_configs:
      - to: 'your-email@gmail.com'
        send_resolved: true
```

> Note: Gmail requires an **App Password**, not your login password.

---

## Importing Grafana Dashboards

### Steps:

1. Open Grafana → [http://localhost:3000](http://localhost:3000)
2. Login:

   * **Username:** admin
   * **Password:** admin
3. Go to **Dashboards → Import**
4. Upload JSON files from the `dashboards/` folder
5. Select **Prometheus** as the data source
6. Save & view dashboards

Included dashboards:

* CPU Usage (%)
* Memory Usage (%)
* Disk Usage (%)

---

##  Testing Alerts (Generate CPU Load)

To simulate high CPU usage:

```bash
for i in $(seq 1 8); do yes > /dev/null & done
```

Stop the load:

```bash
killall yes
```

Alertmanager should send a warning email when CPU pressure is high.

---

## Stopping the Stack

```bash
docker compose down
```

---

## Cleanup (Optional)

```bash
docker system prune -af
```

---

## Technologies Used

* Docker
* Docker Compose
* Prometheus
* Node Exporter
* Grafana
* Alertmanager
* Linux / WSL

---

## Summary

This project provides a full observability stack with metrics collection, dashboards, and alerting. It demonstrates:

* Systems monitoring
* Time-series metrics
* Dashboarding and visualization
* Alerting architecture
* Production-style Docker Compose setup

---

## Author

**Rajkumar R**
Cloud Engineer

---






Feel free to submit PRs or open issues!
