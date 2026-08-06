# Wazuh SOC Home Lab Troubleshooting Guide

## 📌 Overview

This document contains the common issues encountered while building the Wazuh SOC Home Lab and the solutions used to resolve them.

The purpose of this guide is to help others quickly troubleshoot similar deployment and configuration problems.

---

# 🐳 Docker Issues

## Problem

Docker containers failed to start correctly.

### Symptoms

- Containers repeatedly restarting
- Dashboard inaccessible
- Wazuh services unavailable

### Solution

- Verified Docker Desktop was running
- Restarted Docker Desktop
- Restarted containers

```bash
docker compose down
docker compose up -d
docker ps
```

---

# 🐧 Wazuh Agent Disconnected

## Problem

Ubuntu Agent was disconnected from the Wazuh Manager.

### Symptoms

- Agent status showed "Disconnected"
- No events visible in Dashboard

### Solution

Verified agent service.

```bash
sudo systemctl status wazuh-agent
```

Restarted the service.

```bash
sudo systemctl restart wazuh-agent
```

Confirmed the Manager IP address in:

```text
/var/ossec/etc/ossec.conf
```

---

# 🛡️ Suricata Integration

## Problem

Suricata alerts were not appearing in Wazuh.

### Symptoms

- No IDS events
- No Suricata alerts

### Solution

Verified Suricata log file.

```bash
ls /var/log/suricata/
```

Added the following configuration to `ossec.conf`:

```xml
<localfile>
  <log_format>json</log_format>
  <location>/var/log/suricata/eve.json</location>
</localfile>
```

Restarted the Wazuh Agent.

```bash
sudo systemctl restart wazuh-agent
```

---

# 📄 JSON Decoder Error

## Problem

Manager log displayed:

```
Too many fields for JSON decoder.
```

### Cause

Suricata `eve.json` contained more fields than the default decoder could process.

### Status

Issue identified during integration.

Future improvement includes implementing a custom decoder for advanced Suricata events.

---

# 🌐 Ubuntu IP Address Changed

## Problem

Ubuntu VM received a new IP address.

### Previous IP

```
192.168.1.15
```

### Current IP

```
192.168.1.196
```

### Solution

Updated all testing commands to use the new IP address.

Verified connectivity:

```bash
ping 192.168.1.196
```

---

# 🔐 Hydra Wordlist Issue

## Problem

Hydra failed because `rockyou.txt` was missing.

### Error

```
File for passwords not found
```

### Solution

Extracted the compressed wordlist.

```bash
sudo gzip -d /usr/share/wordlists/rockyou.txt.gz
```

Verified:

```bash
ls /usr/share/wordlists/
```

---

# 📂 Git Synchronization Issue

## Problem

Git push failed.

```
Updates were rejected because the remote contains work that you do not have locally.
```

### Solution

Updated the local repository.

```bash
git pull --rebase origin main
git push origin main
```

---

# 🖼️ GitHub Screenshot Issue

## Problem

Images were not displayed in README.

### Cause

Incorrect filename case.

Example:

```
Dashboard.png
```

vs

```
dashboard.png
```

### Solution

Matched the filename exactly with the README image path.

---

# ✅ Best Practices

- Save all documentation locally before committing.
- Use consistent file names.
- Verify Docker containers before troubleshooting.
- Restart the Wazuh Agent after configuration changes.
- Test Suricata integration after every configuration update.
- Commit changes regularly using Git.

---

# 👨‍💻 Author

**Jay Soni**

Network Administrator  
Cyber Security Enthusiast  
SOC Analyst Aspirant

GitHub: https://github.com/JAXY1304
