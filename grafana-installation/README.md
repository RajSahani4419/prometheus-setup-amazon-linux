# Grafana Installation on Amazon Linux 2023

This guide explains how to install and configure Grafana on an EC2 instance running **Amazon Linux 2023**, and how to connect it with Prometheus.

---

## ✅ Prerequisites

- EC2 instance with Amazon Linux 2023
- Prometheus already installed and running
- Port **3000** open in EC2 Security Group (Grafana port)
- Internet access in EC2 instance
- Prometheus should be accessible at `http://localhost:9090`

---

## 🔧 Step-by-Step Installation

###  Step 1: Add Grafana Repository

```bash
sudo nano /etc/yum.repos.d/grafana.repo

```
#### Paste the following:

```ini

[grafana]
name=Grafana OSS
baseurl=https://packages.grafana.com/oss/rpm
repo_gpgcheck=1
enabled=1
gpgcheck=1
gpgkey=https://packages.grafana.com/gpg.key
sslverify=1

````
### Step 2: Install Grafana

```bash

sudo yum install grafana -y

```
#### Step 3: Enable Grafana to Start at Boot

```bash

sudo systemctl enable grafana-server

```

### Step 4: Start Grafana Server

```bash

sudo systemctl start grafana-server

```

### Step 5: Open Port 3000 in Firewall (if firewalld is active)

```bash

sudo firewall-cmd --add-port=3000/tcp --permanent
sudo firewall-cmd --reload

```

#### Step 6: Bind Grafana to Public IP (Optional but recommended)

By default, Grafana binds to 127.0.0.1. To access it from your browser, change it to 0.0.0.0:

```bash

sudo nano /etc/grafana/grafana.ini

```
### Uncomment and change:

```ini

;http_addr =

``` 
### to:

```ini

http_addr = 0.0.0.0

```
### Then restart:

```bash

sudo systemctl restart grafana-server

```
### Step 7: Access Grafana in Browser
Open:

```cpp

http://<your-ec2-public-ip>:3000

```
### Default Username: admin

### Default Password: admin

### You’ll be asked to set a new password.

#### 🔗 Connect Prometheus as Data Source in Grafana
Go to ⚙️ Settings → Data Sources

Click Add data source

Select Prometheus

Enter URL: http://localhost:9090

Click Save & Test

### Done!
Now you can start creating dashboards, visualizing metrics, and connecting other exporters.

---
