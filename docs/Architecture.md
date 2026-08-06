# Wazuh SOC Home Lab Architecture

## 📌 Overview

This document describes the architecture of the Wazuh SOC Home Lab, including the infrastructure, network topology, communication flow, and security monitoring components.

The lab is designed to simulate a small Security Operations Center (SOC) capable of collecting, analyzing, and investigating security events from Linux endpoints.

---

# 🏗️ Lab Architecture

```text
                    Internet
                        │
                        │
             ┌────────────────────┐
             │    Kali Linux      │
             │  Hydra • Nmap      │
             └─────────┬──────────┘
                       │
                192.168.1.0/24
                       │
      ┌────────────────┴────────────────┐
      │                                 │
┌─────▼─────┐                  ┌────────▼─────────┐
│ Ubuntu VM │                  │ Windows 11 Host  │
│ Wazuh     │─────────────────►│ Docker Desktop   │
│ Agent     │                  │ Wazuh Manager    │
│ Suricata  │                  │ Wazuh Dashboard  │
└───────────┘                  │ Wazuh Indexer    │
                               └──────────────────┘
```

---

# 🖥️ Infrastructure

| Component | Description |
|-----------|-------------|
| Windows 11 Host | Runs Docker Desktop and Wazuh containers |
| Docker Desktop | Container platform |
| Wazuh Manager | Collects and analyzes security events |
| Wazuh Dashboard | Web interface for monitoring and investigation |
| Wazuh Indexer | Stores and indexes security events |
| Ubuntu 24.04 | Endpoint monitored by Wazuh Agent |
| Suricata IDS | Network Intrusion Detection System |
| Kali Linux | Attack simulation machine |

---

# 🌐 Network Topology

All systems communicate over the same local network.

Example:

| Device | Role |
|---------|------|
| Windows Host | Docker Platform |
| Ubuntu Server | Monitored Endpoint |
| Kali Linux | Attack Machine |

---

# 🔄 Data Flow

The following flow describes how security events travel through the SOC environment.

```text
Attack
   │
   ▼
Ubuntu Server
   │
   ▼
Suricata IDS
   │
   ▼
Wazuh Agent
   │
   ▼
Wazuh Manager
   │
   ▼
Wazuh Indexer
   │
   ▼
Wazuh Dashboard
```

---

# 📊 Security Monitoring Flow

The SOC lab continuously monitors:

- Authentication Logs
- File Integrity Monitoring (FIM)
- Suricata IDS Events
- SSH Authentication Events
- Linux System Logs

---

# 🧪 Detection Workflow

Example workflow:

1. Kali Linux performs an attack.
2. Ubuntu receives the event.
3. Suricata or Linux logs record the activity.
4. Wazuh Agent forwards the logs.
5. Wazuh Manager analyzes the event.
6. Rules are matched.
7. Alerts are stored in the Indexer.
8. Alerts are displayed in the Dashboard.

---

# 🛡️ Security Components

The architecture includes:

- Wazuh SIEM
- Docker
- Ubuntu Linux
- Kali Linux
- Suricata IDS
- File Integrity Monitoring (FIM)
- SSH Log Monitoring

---

# 📷 Architecture Diagram

Refer to the architecture diagram available in the project README.

---

# 🚀 Future Enhancements

Planned improvements include:

- Active Response
- Windows Sysmon Integration
- Sophos Firewall Integration
- Sigma Rules
- Threat Hunting
- Email Notifications

---

# 👨‍💻 Author

**Jay Soni**

Network Administrator  
Cyber Security Enthusiast  
SOC Analyst Aspirant

GitHub: https://github.com/JAXY1304
