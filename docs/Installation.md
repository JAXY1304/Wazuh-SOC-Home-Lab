# Wazuh SOC Home Lab Installation Guide

## 📌 Overview

This guide explains how to build the Wazuh SOC Home Lab from scratch using Docker, Ubuntu, Kali Linux, and Suricata IDS.

The objective of this lab is to create a practical Security Operations Center (SOC) environment for security monitoring, threat detection, incident investigation, and detection engineering using open-source security tools.

---

# 🖥️ Lab Requirements

## Hardware

- Windows 11 Host
- Minimum 16 GB RAM (8 GB minimum recommended)
- Intel VT-x / AMD-V Virtualization Enabled
- At least 100 GB Free Disk Space

## Software

- Docker Desktop
- VirtualBox
- Ubuntu Server 24.04 LTS
- Kali Linux
- Wazuh 4.12
- Suricata IDS
- Git
- GitHub Account

---

# 🌐 Lab Network

All systems are connected to the same local network.

| Device | Purpose |
|---------|---------|
| Windows 11 | Docker Host |
| Ubuntu Server | Wazuh Agent + Suricata IDS |
| Kali Linux | Attack Machine |
| Docker Containers | Wazuh Manager, Dashboard, Indexer |

---

# 📦 Lab Components

The SOC Lab consists of the following components:

- Docker Engine
- Wazuh Manager
- Wazuh Dashboard
- Wazuh Indexer
- Ubuntu Wazuh Agent
- Suricata IDS
- Kali Linux Attack Machine

---

# 🚀 Installation Steps

## Step 1 – Install Docker Desktop

- Download Docker Desktop
- Enable WSL2 Backend
- Verify installation

```powershell
docker --version
docker compose version
```

---

## Step 2 – Deploy Wazuh

Clone the official Docker deployment.

```bash
git clone https://github.com/wazuh/wazuh-docker.git
cd wazuh-docker/single-node
docker compose up -d
```

Verify containers:

```bash
docker ps
```

---

## Step 3 – Create Ubuntu Virtual Machine

Install:

- Ubuntu Server 24.04
- OpenSSH Server

Update packages:

```bash
sudo apt update
sudo apt upgrade -y
```

---

## Step 4 – Install Wazuh Agent

Download and install the Wazuh Agent.

Configure:

- Manager IP Address
- Agent Name

Enable service:

```bash
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

Verify:

```bash
sudo systemctl status wazuh-agent
```

---

## Step 5 – Install Suricata IDS

Install Suricata:

```bash
sudo apt install suricata -y
```

Enable service:

```bash
sudo systemctl enable suricata
sudo systemctl start suricata
```

Verify:

```bash
sudo systemctl status suricata
```

---

## Step 6 – Configure Wazuh

Configure the Wazuh Agent to monitor:

- Authentication Logs
- File Integrity Monitoring
- Suricata Logs

Add Suricata log monitoring:

```xml
<localfile>
  <log_format>json</log_format>
  <location>/var/log/suricata/eve.json</location>
</localfile>
```

Restart the agent:

```bash
sudo systemctl restart wazuh-agent
```

---

## Step 7 – Configure Kali Linux

Install:

- Nmap
- Hydra

Verify:

```bash
nmap --version
hydra -h
```

---

# 🧪 Validation Tests

Perform the following tests to validate the lab.

## File Integrity Monitoring

Create a file:

```bash
touch ~/FIM-Test.txt
```

Expected Result:

- File Creation Alert
- File Deletion Alert

---

## SSH Brute Force

Example:

```bash
hydra -l admin -P rockyou.txt ssh://<TARGET-IP>
```

Expected Result:

- Failed SSH Authentication
- Maximum Authentication Attempts Exceeded

---

## Nmap Service Enumeration

Example:

```bash
nmap -sV <TARGET-IP>
```

Expected Result:

- SSH Port Detection
- Service Enumeration

---

## Suricata IDS

Generate network traffic.

Expected Result:

- Suricata Alerts
- IDS Events
- Dashboard Events

---

# ✅ Verification Checklist

Confirm the following:

- Docker containers are running.
- Wazuh Dashboard is accessible.
- Ubuntu Agent is connected.
- Kali Linux can communicate with Ubuntu.
- Suricata is generating alerts.
- File Integrity Monitoring is working.
- SSH authentication events are visible.
- Security events appear in Wazuh Dashboard.

---

# 📷 Expected Screenshots

The following screenshots should be available after installation.

- Wazuh Dashboard
- Wazuh Agents
- File Integrity Monitoring
- SSH Brute Force Detection
- Suricata Alerts

---

# 📚 References

- Wazuh Documentation
- Suricata Documentation
- Docker Documentation
- Ubuntu Documentation

---

# 🚀 Next Steps

After completing the installation, continue with:

- File Integrity Monitoring
- SSH Brute Force Detection
- Suricata Integration
- Nmap Enumeration
- Active Response
- Windows Sysmon Integration
- Sophos Firewall Integration
- Threat Hunting
- Sigma Rules
- Incident Investigation

---

## 👨‍💻 Author

**Jay Soni**

Network Administrator  
Cyber Security Enthusiast  
SOC Analyst Aspirant

GitHub: https://github.com/JAXY1304
