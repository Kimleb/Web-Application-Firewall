# 🛡️ SafeLine WAF Home Lab — Project Guide

A cybersecurity home lab simulating real-world web attacks using **DVWA** as the target, **SafeLine WAF** as the firewall, and **Wazuh** for SIEM alerting.

---

## 🖥️ Environment

| Component | Detail |
|-----------|--------|
| OS | Ubuntu Server 24.04 LTS |
| Web Server | Apache2 |
| Database | MySQL 8.0 |
| WAF | SafeLine CE (Docker) |
| Target App | DVWA |
| SIEM | Wazuh |
| Hostname | `webserver.thesocialdork` |

---

## 🌐 Access URLs

| Service | URL |
|---------|-----|
| DVWA | `http://webserver.thesocialdork/dvwa` |
| SafeLine Console | `https://webserver.thesocialdork:9443` |

---

## ⚡ Quick Reference Commands

### SafeLine WAF
```bash
# Start
cd /data/safeline && sudo docker compose up -d

# Stop
cd /data/safeline && sudo docker compose down

# Check status
sudo docker compose ps

# Reset admin password
sudo docker exec safeline-mgt /app/mgt-cli reset-admin
```

### DVWA
```bash
# Restart web server
sudo systemctl restart apache2

# Check error logs
sudo tail -50 /var/log/apache2/error.log
```

### Wazuh
```bash
# Agent status
sudo systemctl status wazuh-agent

# Live alerts
sudo tail -f /var/ossec/logs/alerts/alerts.log
```

---

## 🔑 Lab Credentials

> ⚠️ For lab use only — never use in production.

| Service | Username | Password |
|---------|----------|----------|
| DVWA Login | `admin` | `password` |
| DVWA MySQL | `dvwa` | `p@ssw0rd` |
| SafeLine | `admin` | *(generated on reset)* |

---

## 🔥 Attack Scenarios

| Attack | DVWA Module | SafeLine | Wazuh |
|--------|------------|----------|-------|
| SQL Injection | SQL Injection | ✅ Blocks | ✅ Alerts |
| XSS | XSS Reflected/Stored | ✅ Blocks | ✅ Alerts |
| Command Injection | Command Injection | ✅ Blocks | ✅ Alerts |
| File Inclusion | File Inclusion | ✅ Blocks | ✅ Alerts |
| Brute Force | Brute Force | ✅ Blocks | ✅ Alerts |

---

## 🗂️ Project Structure

```
├── README.md
├── configs/          # Apache, DVWA, SafeLine config files
├── docs/             # Step-by-step setup guides
├── scripts/          # Automation scripts
└── screenshots/      # Evidence of attacks and detections
```

---

## ⚠️ Common Issues & Fixes

**DVWA 500 error on setup**
→ MySQL user doesn't exist. Create it:
```bash
sudo mysql -e "CREATE USER 'dvwa'@'127.0.0.1' IDENTIFIED BY 'p@ssw0rd'; GRANT ALL ON dvwa.* TO 'dvwa'@'127.0.0.1'; FLUSH PRIVILEGES;"
```

**SafeLine containers not starting**
→ containerd storage corrupted. Fix:
```bash
sudo systemctl stop docker containerd
sudo rm -rf /var/lib/containerd
sudo systemctl start containerd docker
```

**SafeLine installer loops on directory prompt**
→ Run directly in terminal as sudo — never pipe input to the script.

**DVWA not accessible from browser**
→ Add to `/etc/hosts` on your machine:
```
10.160.59.53    webserver.thesocialdork
```

---

## 📸 Screenshots
See `/screenshots` folder for evidence of:
- SafeLine blocking SQL injection and XSS attacks
- Wazuh alert dashboards
- DVWA attack inputs and WAF responses

---

*For educational purposes only. Only test against systems you own or have permission to test.*
