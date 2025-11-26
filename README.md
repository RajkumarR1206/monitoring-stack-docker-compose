---

#  Monitoring Stack using Docker Compose

### **Prometheus + Grafana + Node Exporter (Localhost Monitoring Setup)**

This project sets up a complete monitoring and observability stack on your local machine using **Docker Compose**.
It uses **Prometheus** to collect metrics, **Node Exporter** to gather system metrics, and **Grafana** to visualize them.


---

## Features

* Full monitoring setup using `docker-compose`
* Grafana dashboards for CPU, Memory & Disk
* Prometheus scraping Node Exporter metrics
* 100% local — runs on WSL2 / Ubuntu / Docker Desktop
* Easy to extend with alerts, exporters, integrations

---

## Project Structure

```
monitoring-stack/
├── docker-compose.yml
├── grafana/
├── prometheus/
│   └── prometheus.yml
├── dashboards/
│   ├── cpu_dashboard.json
│   ├── memory_dashboard.json
│   └── disk_dashboard.json
└── README.md
```

---

##  Prerequisites

Ensure the following are installed:

* Docker Desktop (Windows/Mac/Linux)
* WSL2 + Ubuntu (Windows users)
* Git

---

##  Getting Started

### 1.  Clone the Repository

```bash
git clone https://github.com/RajkumarR1206/monitoring-stack-docker-compose.git
cd monitoring-stack-docker-compose
```

### 2.  Start All Services

```bash
docker compose up -d
```

This starts:

| Service       | URL                                            |
| ------------- | ---------------------------------------------- |
| Prometheus    | [http://localhost:9090](http://localhost:9090) |
| Grafana       | [http://localhost:3000](http://localhost:3000) |
| Node Exporter | [http://localhost:9100](http://localhost:9100) |

---

## 3. Import Grafana Dashboards

1. Open Grafana → [http://localhost:3000](http://localhost:3000)
   Login: **admin / admin**
2. Go to **Dashboards → Import**
3. Upload any `.json` file from the `dashboards/` folder
4. Select **Prometheus** as the datasource
5. Click **Import**

Included dashboards:

* CPU Usage
* Memory Usage
* Disk Activity

---

## Prometheus Configuration (`prometheus.yml`)

Prometheus automatically scrapes Node Exporter using:

```yaml
scrape_configs:
  - job_name: "node_exporter"
    static_configs:
      - targets: ["node-exporter:9100"]
```

---

## Technologies Used

* **Docker**
* **Docker Compose**
* **Prometheus**
* **Grafana**
* **Node Exporter**
* **Linux / WSL2**

---

This project demonstrates real-world capabilities:

* Monitoring fundamentals
* Metrics scraping & visualization
* Docker-based infra provisioning
* Grafana dashboard creation
* Observability 
* Local production-style monitoring setup

---

##  Screenshots


```
/screenshots
    cpu_dashboard.png
    memory_dashboard.png
    disk_dashboard.png
```

---

## Author

**Rajkumar R**
Cloud Engineer

---

## Feedback / Improvements

Feel free to submit PRs or open issues!
