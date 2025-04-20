# Prometheus Installation on Amazon Linux 2023

This guide walks through the complete Prometheus installation steps on an EC2 instance running **Amazon Linux 2023**.

---

## Prerequisites

- Amazon EC2 Instance (Amazon Linux 2023)
- SSH access to EC2 instance
- Port 9090 open in the EC2 Security Group
- Basic Linux command line knowledge

---

## Step-by-step Installation

### Step 1: Update your system

```bash

sudo yum update -y

```
#### Step 2: Create a prometheus user

```bash


sudo useradd --no-create-home --shell /bin/false prometheus

```
### Step 3: Create directories for Prometheus config and data

```bash


sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus

```
#### Step 4: Download Prometheus tar file

```bash

cd /opt
wget https://github.com/prometheus/prometheus/releases/download/v2.52.0/prometheus-2.52.0.linux-amd64.tar.gz

```
### Step 5: Extract the tar file

```bash


tar -xvf prometheus-2.52.0.linux-amd64.tar.gz

```
#### Step 6: Rename the extracted folder

```bash

mv prometheus-2.52.0.linux-amd64 prometheus

```
#### Step 7: Create prometheus.yml config file

```bash

cd /opt/prometheus
sudo nano prometheus.yml

```
#### Paste the following:

```yaml

global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ['localhost:9090']

  - job_name: "node_exporter"
    static_configs:
      - targets: ['localhost:9100']

```

#### Step 8: Create Prometheus systemd service file

```bash

sudo nano /etc/systemd/system/prometheus.service

```
#### Paste the following:

```ini

[Unit]
Description=Prometheus Monitoring System
Documentation=https://prometheus.io/docs/introduction/overview/
After=network.target

[Service]
User=ec2-user
ExecStart=/opt/prometheus/prometheus \
  --config.file=/opt/prometheus/prometheus.yml \
  --storage.tsdb.path=/opt/prometheus/data \
  --web.listen-address=:9090
Restart=always
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target

```

#### Step 9: Start and enable the service

```bash

sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl enable prometheus

```

#### Prometheus Web UI

Open your browser and visit:
🔗 http://<your-ec2-public-ip>:9090

# Prometheus is now successfully installed and running on Amazon Linux 2023.

---
