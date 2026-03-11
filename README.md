# Prometheus Observability Stack

A production-ready monitoring and alerting stack deployed using Docker Compose on AWS EC2, provisioned with Terraform.

## Stack

| Tool | Purpose | Port |
|------|---------|------|
| Prometheus | Metrics collection & storage | 9090 |
| Grafana | Visualization & dashboards | 3000 |
| Node Exporter | Host metrics (CPU, RAM, Disk) | 9100 |
| Alertmanager | Alert routing (Email & Slack) | 9093 |

## Architecture

```
AWS EC2 (Terraform)
└── Docker Network: monitor
    ├── Prometheus (scrapes metrics, evaluates alert rules)
    │   ├── Node Exporter (host metrics)
    │   └── Alertmanager (fires alerts)
    └── Grafana (visualizes Prometheus data)
```

## Project Structure

```
.
├── prometheus/
│   ├── prometheus.yml       # Scrape configs & alerting rules reference
│   ├── alertrules.yml       # Alert rule definitions
│   └── targets.json         # Service discovery targets
├── alertmanager/
│   └── alertmanager.yml     # Email & Slack alert routing
├── terraform-aws/
│   ├── modules/             # Reusable Terraform modules
│   ├── prometheus-stack/    # Main stack provisioning
│   └── vars/                # Variable definitions
├── docker-compose.yml       # Full stack definition
└── Makefile                 # Service discovery automation
```

## Quick Start

### Prerequisites
- Docker & Docker Compose
- Make

### Run locally

```bash
git clone https://github.com/Shubhamharanale7/Prometheus-Observability-Stack.git
cd Prometheus-Observability-Stack
docker compose up -d
```

Access:
- Prometheus: `http://localhost:9090`
- Grafana: `http://localhost:3000`
- Alertmanager: `http://localhost:9093`
- Node Exporter: `http://localhost:9100`

## Deploy to AWS EC2 with Terraform

```bash
cd terraform-aws/prometheus-stack
terraform init
terraform plan
terraform apply --auto-approve
```

Once EC2 is up, SSH in and run:

```bash
docker compose up -d
```

### Service Discovery (Public IP)

The Makefile automates replacing `127.0.0.1` with your EC2 public IP:

```bash
make service-discovery
```

To rollback to localhost:

```bash
make rollback
```

## Alerting Setup

### Email Alerts
Edit `alertmanager/alertmanager.yml`:
```yaml
receivers:
- name: 'email-notifications'
  email_configs:
    - to: 'your-email@example.com'
      from: 'prometheus@example.com'
      smarthost: 'smtp.gmail.com:587'
      auth_username: 'your-username'
      auth_password: 'your-app-password'
      send_resolved: true
```

### Slack Alerts
```yaml
- name: 'slack-notifications'
  slack_configs:
    - api_url: "https://hooks.slack.com/services/YOUR-WEBHOOK"
      channel: '#alerts'
      send_resolved: true
```

## Prometheus Configuration

- **Scrape interval:** 15s
- **Retention:** 7 days
- **Service discovery:** File-based (`targets.json`)
- **Alert rules:** Loaded from `alertrules.yml`

## Grafana Dashboards

After login (admin/admin), import these community dashboards:

| Dashboard | ID |
|-----------|-----|
| Node Exporter Full | 1860 |
| Prometheus Stats | 2 |

## Tech Stack

- **Docker & Docker Compose** — containerized deployment
- **Prometheus** — metrics scraping and storage
- **Grafana** — visualization
- **Node Exporter** — host-level metrics
- **Alertmanager** — alert routing via Email and Slack
- **Terraform** — AWS EC2 provisioning
- **Make** — automation scripts

---

**Built by [Shubham Haranale](https://github.com/Shubhamharanale7)**

[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Shubhamharanale7)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/shubhamharanale7)
## Prometheus Observability Stack Using Docker 


![image](https://github.com/techiescamp/devops-projects/assets/106984297/6a9f9d90-e56f-4038-82c7-e8eae29d8bcf)


## Project Documentation.

Refer [Setting Up Prometheus Observability Stack](https://devopscube.com/setup-prometheus-using-docker/) for the entire setup walkthrough.

