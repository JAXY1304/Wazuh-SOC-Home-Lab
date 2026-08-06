# Wazuh SOC Home Lab Installation Guide

## 📌 Overview

This guide explains how to build the Wazuh SOC Home Lab from scratch using Docker, Ubuntu, Kali Linux, and Suricata IDS.

The objective is to create a practical Security Operations Center (SOC) lab for security monitoring, threat detection, incident investigation, and detection engineering.

---

## 🖥️ Lab Requirements

- Windows 11 Host
- Docker Desktop
- Ubuntu 24.04 Virtual Machine
- Kali Linux Virtual Machine
- Wazuh 4.12
- Suricata IDS
- Internet Connection

---

## 🌐 Network Topology

All systems are connected to the same local network.

| Device | Purpose |
|---------|---------|
| Windows 11 | Docker Host |
| Ubuntu 24.04 | Wazuh Agent + Suricata |
| Kali Linux | Attack Machine |

---

## 📦 Components

- Wazuh Manager
- Wazuh Dashboard
- Wazuh Indexer
- Ubuntu Wazuh Agent
- Suricata IDS

---

## ✅ Verification

The installation is considered successful when:

- Wazuh Dashboard is accessible.
- Ubuntu Agent is connected.
- Suricata is generating alerts.
- File Integrity Monitoring (FIM) is working.
- SSH authentication events are visible in Wazuh.

---

## 🚀 Next Steps

After completing the installation:

- Configure File Integrity Monitoring (FIM)
- Perform SSH Brute Force simulation
- Integrate Suricata IDS
- Perform Nmap service enumeration
- Investigate alerts using Wazuh Dashboard
