# 🛡️ SafeLine WAF Home Lab

> A hands-on home lab documenting the deployment and integration of SafeLine WAF, DVWA, and Wazuh SIEM for real-world web application attack simulation and detection.

---

## 📌 Project Overview

This project documents the end-to-end setup of a cybersecurity home lab environment built on Ubuntu Server. The lab simulates real-world web application attack scenarios using **DVWA** (Damn Vulnerable Web Application) as the target, **SafeLine WAF** as the web application firewall, and **Wazuh** as the SIEM for log aggregation and threat detection.

The goal is to demonstrate how modern WAF solutions detect and block common web attacks such as SQL Injection, XSS, Command Injection, and File Inclusion — while correlating security events in Wazuh for centralized visibility.

---

## 🏗️ Lab Architecture

```
[ Attack Machine ] ──────────────────────────────────────────────────────────┐
                                                                              │
                                                              HTTP/HTTPS      │
                                                                              ▼
                                                         ┌─────────────────────────┐
                                                         │     SafeLine WAF        │
                                                         │   (Port 9443 Mgmt)      │
                                                         │   Docker Container      │
                                                         └──────────┬──────────────┘
                                                                    │
                                                              Proxied Traffic
                                                                    │
                                                                    ▼
                                                         ┌─────────────────────────┐
                                                         │   Ubuntu Server         │
                                                         │   (webserver.thesocialdork) │
                                                         │                         │
                                                         │  ┌───────────────────┐  │
                                                         │  │  Apache2 (Port 80)│  │
                                                         │  │  DVWA Application │  │
                                                         │  └───────────────────┘  │
                                                         │                         │
                                                         │  ┌───────────────────┐  │
                                                         │  │  MySQL 8.0        │  │
                                                         │  │  DVWA Database    │  │
                                                         │  └───────────────────┘  │
                                                         └──────────┬──────────────┘
                                                                    │
                                                              Log Forwarding
                                                                    │
                                                                    ▼
                                                         ┌─────────────────────────┐
                                                         │     Wazuh SIEM          │
                                                         │  (Log Aggregation &     │
                                                         │   Threat Detection)     │
                                                         └─────────────────────────┘
```

---

## 🖥️ Environment Details

| Component | Details |
|-----------|---------|
| **OS** | Ubuntu Server 24.04 LTS |
| **Server IP** | `10.160.59.53` |
| **Hostname** | `webserver.thesocialdork` |
| **Web Server** | Apache2 2.4.58 |
| **PHP** | 8.3.6 |
| **Database** | MySQL 8.0.45 |
| **WAF** | SafeLine CE v9.3.3 (Docker) |
| **SIEM** | Wazuh |
| **DVWA** | Latest (Git) |

---

## 📁 Repository Structure

```
safeline-waf-project/
├── README.md                        # This file
├── configs/
│   ├── dvwa-config.inc.php          # DVWA database config
│   ├── safeline-docker-compose.yaml # SafeLine Docker Compose
│   ├── safeline.env                 # SafeLine environment variables
│   └── dvwa.conf                   # Apache VirtualHost config
├── docs/
│   ├── 01-environment-setup.md      # Server & prerequisites setup
│   ├── 02-dvwa-installation.md      # DVWA installation guide
│   ├── 03-safeline-installation.md  # SafeLine WAF setup guide
│   ├── 04-wazuh-integration.md      # Wazuh SIEM integration
│   ├── 05-attack-simulations.md     # Attack scenarios & results
│   └── 06-troubleshooting.md        # Common issues & fixes
├── scripts/
│   ├── setup-dvwa.sh                # Automated DVWA setup script
│   ├── setup-safeline.sh            # SafeLine install helper
│   └── reset-dvwa-db.sh             # Reset DVWA database
└── screenshots/
    ├── safeline/                     # SafeLine dashboard screenshots
    ├── dvwa/                         # DVWA screenshots
    ├── wazuh/                        # Wazuh alerts screenshots
    └── attacks/                      # Attack simulation screenshots
```

---

## 🚀 Quick Start

### Prerequisites
- Ubuntu Server 24.04 LTS
- Docker & Docker Compose installed
- Apache2, PHP 8.3, MySQL 8.0
- Minimum 4GB RAM, 55GB disk space

### 1. Clone this repository
```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
cd safeline-waf-project
```

### 2. Follow the docs in order
```
docs/01-environment-setup.md
docs/02-dvwa-installation.md
docs/03-safeline-installation.md
docs/04-wazuh-integration.md
docs/05-attack-simulations.md
```

---

## 📖 Documentation

### [01 - Environment Setup](docs/01-environment-setup.md)
- Ubuntu Server configuration
- Installing Docker, Apache2, PHP, MySQL
- Firewall (UFW) rules
- Hostname configuration (`/etc/hosts`)

### [02 - DVWA Installation](docs/02-dvwa-installation.md)
- Cloning DVWA from GitHub
- Apache VirtualHost configuration
- MySQL database and user setup
- PHP configuration for labs

### [03 - SafeLine WAF Setup](docs/03-safeline-installation.md)
- Docker-based SafeLine CE installation
- Management console access (port 9443)
- Admin password reset
- Adding DVWA as a protected site

### [04 - Wazuh SIEM Integration](docs/04-wazuh-integration.md)
- Wazuh agent installation
- Log forwarding configuration
- Custom rules for WAF alerts
- Dashboard setup

### [05 - Attack Simulations](docs/05-attack-simulations.md)
- SQL Injection attacks
- Cross-Site Scripting (XSS)
- Command Injection
- File Inclusion
- WAF detection & blocking results

---

## 🔥 Attack Scenarios Covered

| Attack Type | DVWA Module | SafeLine Detection | Wazuh Alert |
|------------|-------------|-------------------|-------------|
| SQL Injection | SQL Injection | ✅ Blocked | ✅ Logged |
| Blind SQLi | SQL Injection (Blind) | ✅ Blocked | ✅ Logged |
| XSS (Reflected) | XSS (Reflected) | ✅ Blocked | ✅ Logged |
| XSS (Stored) | XSS (Stored) | ✅ Blocked | ✅ Logged |
| Command Injection | Command Injection | ✅ Blocked | ✅ Logged |
| File Inclusion | File Inclusion | ✅ Blocked | ✅ Logged |
| Brute Force | Brute Force | ✅ Blocked | ✅ Logged |

---

## 🔑 Default Credentials

> ⚠️ **These are lab credentials only. Never use in production.**

| Service | Username | Password |
|---------|----------|----------|
| DVWA Web Login | `admin` | `password` |
| DVWA MySQL User | `dvwa` | `p@ssw0rd` |
| SafeLine WAF | `admin` | *(reset on first login)* |

---

## 🛠️ Key Commands Reference

### SafeLine WAF
```bash
# Start SafeLine
cd /data/safeline && sudo docker compose up -d

# Stop SafeLine
cd /data/safeline && sudo docker compose down

# Check container status
sudo docker compose ps

# Reset admin password
sudo docker exec safeline-mgt /app/mgt-cli reset-admin

# View logs
sudo docker logs safeline-mgt
```

### DVWA
```bash
# Restart Apache
sudo systemctl restart apache2

# Reset DVWA database
sudo mysql -u dvwa -p'p@ssw0rd' -h 127.0.0.1 -e "DROP DATABASE dvwa; CREATE DATABASE dvwa;"

# View Apache error logs
sudo tail -50 /var/log/apache2/error.log
```

### Wazuh
```bash
# Check Wazuh agent status
sudo systemctl status wazuh-agent

# Restart Wazuh agent
sudo systemctl restart wazuh-agent

# View Wazuh alerts
sudo tail -f /var/ossec/logs/alerts/alerts.log
```

---

## 🌐 Access URLs

| Service | URL |
|---------|-----|
| DVWA | `http://webserver.thesocialdork/dvwa` |
| DVWA Setup | `http://webserver.thesocialdork/dvwa/setup.php` |
| SafeLine WAF | `https://webserver.thesocialdork:9443` |

---

## 🐛 Troubleshooting

See [docs/06-troubleshooting.md](docs/06-troubleshooting.md) for common issues including:
- Docker daemon not running
- containerd metadata.db missing
- DVWA 500 errors (MySQL access denied)
- SafeLine image pull failures
- Apache VirtualHost loops

---

## 📸 Screenshots

Screenshots are organized in the `/screenshots` directory:
- `screenshots/safeline/` — WAF dashboard, blocked attacks, rules
- `screenshots/dvwa/` — DVWA modules, attack inputs
- `screenshots/wazuh/` — Alert dashboards, event logs
- `screenshots/attacks/` — Attack payloads and WAF responses

---

## 🏷️ Tags

`waf` `dvwa` `wazuh` `safeline` `siem` `cybersecurity` `homelab` `web-application-security` `docker` `ubuntu` `penetration-testing` `threat-detection` `sql-injection` `xss`

---

## 📄 License

This project is for educational purposes only. Do not use against systems you do not own or have explicit permission to test.

---

*Built with 🔐 by [YOUR NAME] | Home Lab Security Documentation*
