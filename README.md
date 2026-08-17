🛡️ Wazuh SOC Home Lab

Enterprise-style Security Operations Center (SOC) Home Lab built using Wazuh SIEM, Docker, Ubuntu, Kali Linux, and Suricata IDS for threat detection, security monitoring, incident investigation, and automated response.

📌 Overview

This project demonstrates the design and implementation of a practical SOC environment using open-source security technologies.

The lab simulates real-world security scenarios against a monitored Ubuntu endpoint and demonstrates how security events can be generated, collected, detected, investigated, correlated, and responded to automatically.

The primary SIEM platform used in this project is Wazuh.

The lab includes attack simulations using Kali Linux, endpoint monitoring through Wazuh Agent, network intrusion detection using Suricata, and automated containment using Wazuh Active Response.

🏗️ Lab Architecture

                              Internet
                                  │
                                  │
                         ┌────────▼────────┐
                         │   Kali Linux    │
                         │ Hydra • Nmap    │
                         │ SSH Testing     │
                         └────────┬────────┘
                                  │
                           192.168.1.0/24
                                  │
                  ┌───────────────┴───────────────┐
                  │                               │
          ┌───────▼────────┐             ┌────────▼─────────┐
          │   Ubuntu VM    │             │   Docker Host    │
          │                │             │                  │
          │ Wazuh Agent    │────────────►│ Wazuh Manager    │
          │ Suricata IDS   │             │ Wazuh Indexer    │
          │ SSH Service    │             │ Wazuh Dashboard  │
          │ FIM            │             │                  │
          └────────────────┘             └──────────────────┘
                  │
                  ▼
           Security Events
                  │
                  ▼
          Wazuh Detection Engine
                  │
                  ▼
          Active Response
                  │
                  ▼
          Firewall / iptables

🎯 Project Objectives

Build a functional SIEM environment using Wazuh

Monitor Linux endpoints

Collect and analyze security logs

Simulate real-world attacks in a controlled environment

Detect SSH authentication attacks

Detect file integrity changes

Monitor network reconnaissance

Integrate Suricata IDS telemetry

Investigate security alerts

Implement automated incident response

Automatically block malicious source IP addresses

Demonstrate detection-to-response workflow

Map security activity to MITRE ATT&CK concepts

🚀 Features

Docker-based Wazuh deployment

Ubuntu endpoint monitoring

Kali Linux attack simulation

File Integrity Monitoring (FIM)

SSH Brute Force Detection

Nmap Network Reconnaissance

Suricata IDS Integration

EVE JSON Log Monitoring

Security Event Investigation

Wazuh Active Response

Automatic IP Blocking

Firewall / iptables Integration

MITRE ATT&CK Mapping

Detection-to-Response Workflow

🧪 Detection Labs

Detection / Capability

Status

Wazuh SIEM Installation

✅ Completed

Ubuntu Agent Monitoring

✅ Completed

Kali Linux Attack Simulation

✅ Completed

File Integrity Monitoring

✅ Completed

SSH Brute Force Detection

✅ Completed

Nmap Detection

✅ Completed

Suricata IDS Integration

✅ Completed

Wazuh Active Response

✅ Completed

Automatic IP Blocking

✅ Completed

Threat Hunting Dashboard

🚧 Planned

Windows Sysmon

📅 Planned

Sophos Firewall Integration

📅 Planned

Sigma Rules

📅 Planned

Email Alerting

📅 Planned

🛠️ Technologies

Category

Technology

SIEM

Wazuh 4.12.0

Containerization

Docker

Endpoint OS

Ubuntu 24.04

Attack Machine

Kali Linux

IDS

Suricata 7.0.3

Reconnaissance

Nmap

Password Testing

Hydra

Endpoint Monitoring

Wazuh Agent

Firewall Response

iptables / firewall-drop

Log Format

EVE JSON

Framework

MITRE ATT&CK

Version Control

Git / GitHub

🔍 Security Monitoring Capabilities

The lab currently provides visibility into:

SSH authentication failures

Invalid SSH users

Repeated authentication failures

File integrity changes

Network reconnaissance

TCP network activity

Source IP addresses

Destination IP addresses

Source and destination ports

Suricata IDS events

EVE JSON telemetry

Wazuh alerts

Automated firewall response

Security event investigation

🔐 Detection 1 — File Integrity Monitoring

Wazuh File Integrity Monitoring (FIM) is used to detect unauthorized or unexpected changes to monitored files.

Capabilities

File modification detection

File creation detection

File deletion detection

File integrity monitoring

Security event investigation

Documentation

detections/FIM.md

🔑 Detection 2 — SSH Brute Force

A controlled SSH authentication attack was performed from Kali Linux against the Ubuntu endpoint.

Attack Environment

Attacker:
Kali Linux
192.168.1.111

Target:
Ubuntu-01
192.168.1.196

Service:
SSH / TCP 22

A non-existent username was used:

wronguser

Wazuh Detection

The attack generated Wazuh authentication alerts.

Rule ID

Level

Detection

5710

5

SSH login using a non-existent user

5503

5

PAM user login failed

2502

10

Repeated password failures

Documentation

detections/SSH-BruteForce.md

🌐 Detection 3 — Nmap Reconnaissance

A controlled Nmap SYN scan was performed from Kali Linux against Ubuntu-01.

Scan

nmap -sS -T4 192.168.1.196

Result

Nmap scan report for 192.168.1.196

Host is up

PORT   STATE SERVICE
22/tcp open  ssh

The scan demonstrated:

Network reconnaissance

Service enumeration

Open port discovery

Attacker-to-target network activity

SOC visibility into reconnaissance behavior

Documentation

detections/Nmap.md

🛡️ Detection 4 — Suricata IDS Integration

Suricata 7.0.3 was deployed on the Ubuntu endpoint as an IDS.

Suricata was verified as running successfully:

suricata.service - Suricata IDS/IDP daemon
Active: active (running)
Version: 7.0.3

Interface

enp0s3

EVE JSON Log

/var/log/suricata/eve.json

Suricata generated EVE JSON telemetry containing network security events.

Observed telemetry includes:

TCP activity

Source IP

Destination IP

Source port

Destination port

Protocol

Application protocol

Suricata alerts

Network flow information

The Nmap simulation generated observable network traffic between:

Source:
192.168.1.111

Destination:
192.168.1.196

The latest Nmap scan demonstrated network visibility, although a dedicated Nmap-specific Suricata alert was not observed for that scan.

Documentation

detections/Suricata.md

⚡ Detection 5 — Wazuh Active Response

The lab successfully demonstrates automated incident response.

Wazuh was configured to execute the firewall-drop Active Response when Rule 5710 is triggered.

Active Response Configuration

<active-response>
  <disabled>no</disabled>
  <command>firewall-drop</command>
  <location>local</location>
  <rules_id>5710</rules_id>
</active-response>

The configuration was validated successfully using:

wazuh-analysisd -t
RC=0

Attack Simulation

A controlled SSH authentication test was performed from Kali Linux against Ubuntu-01 using a non-existent username:

ssh wronguser@192.168.1.196

Attacker IP:

192.168.1.111

Detection

Wazuh generated Rule 5710:

Rule ID: 5710
Level: 5
Description: sshd: Attempt to login using a non-existent user
Source IP: 192.168.1.111
Username: wronguser

Ubuntu recorded:

Invalid user wronguser from 192.168.1.111

Active Response Evidence

The Wazuh Active Response log confirmed execution of firewall-drop:

active-response/bin/firewall-drop: Starting
command: add
rule.id: 5710
srcip: 192.168.1.111
srcuser: wronguser
program: active-response/bin/firewall-drop
command: check_keys
keys: ["192.168.1.111"]
active-response/bin/firewall-drop: Ended

Firewall Evidence

After the detection, Ubuntu iptables showed:

Chain INPUT (policy ACCEPT)

1    DROP    0    --    192.168.1.111    0.0.0.0/0

This confirms that the attacker IP was automatically blocked on the Ubuntu endpoint.

Detection and Response Flow

Kali Linux
192.168.1.111
      |
      | SSH Invalid User Attempt
      v
Ubuntu-01
192.168.1.196
      |
      | Wazuh Rule 5710
      v
Wazuh Manager
      |
      | Active Response
      v
firewall-drop
      |
      v
Ubuntu iptables
      |
      +── DROP 192.168.1.111

This demonstrates a complete:

Detection → Investigation → Automated Containment

workflow.

Documentation

detections/Active-Response.md

🔄 SOC Detection & Response Workflow

The overall workflow implemented in this lab is:

                 Attack Simulation
                        |
                        v
              Kali Linux / Network
                        |
                        v
                 Ubuntu Endpoint
                        |
              +---------+---------+
              |                   |
              v                   v
         Wazuh Agent          Suricata IDS
              |                   |
              +---------+---------+
                        |
                        v
                  Wazuh Manager
                        |
                        v
                 Detection Rules
                        |
                        v
                      Alert
                        |
                        v
                  Investigation
                        |
                        v
                  Active Response
                        |
                        v
                  Firewall Blocking
                        |
                        v
                    Containment

🧠 SOC Investigation Approach

1. Identify

Determine:

Source IP

Destination IP

Username

Port

Protocol

Timestamp

Detection Rule

2. Analyze

Review:

Wazuh alerts

Endpoint logs

Authentication logs

Suricata EVE JSON

Firewall activity

Attack behavior

3. Correlate

Correlate:

Attack Activity
      +
Endpoint Logs
      +
Wazuh Alert
      +
Network Telemetry

4. Respond

When configured, Wazuh Active Response can automatically:

Detect
  |
  v
Trigger Rule
  |
  v
Execute Response
  |
  v
Block Source IP

5. Document

Each detection is documented under:

detections/

📂 Project Structure

Wazuh-SOC-Home-Lab/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── docs/
│   ├── Installation.md
│   ├── Architecture.md
│   └── Troubleshooting.md
│
├── detections/
│   ├── FIM.md
│   ├── SSH-BruteForce.md
│   ├── Nmap.md
│   ├── Suricata.md
│   └── Active-Response.md
│
├── screenshots/
│   ├── Dashboard.png
│   ├── agents.png
│   ├── FIM.png
│   ├── SSH-BruteForce.png
│   └── Suricata.png
│
├── configs/
│
└── assets/

🎯 Skills Demonstrated

Security Operations

SOC Monitoring

Security Event Analysis

Alert Investigation

Incident Investigation

Detection Engineering

Automated Incident Response

Network Security

Network Reconnaissance

Nmap

SSH Security

TCP/IP Analysis

Network Traffic Monitoring

IDS Monitoring

SIEM

Wazuh

Wazuh Agents

Wazuh Rules

Log Collection

Alert Analysis

Active Response

Endpoint Security

File Integrity Monitoring

Linux Security

Authentication Monitoring

Firewall Management

iptables

IDS

Suricata

EVE JSON

Network Telemetry

IDS Alert Analysis

Frameworks

MITRE ATT&CK

SOC Detection Concepts

Detection-to-Response Workflow

🧩 MITRE ATT&CK Coverage

The lab activities support investigation of techniques related to:

Activity

ATT&CK Area

SSH Authentication Attack

Credential Access / Remote Services

Nmap Scanning

Network Service Scanning

File Integrity Monitoring

File and Directory Monitoring

Network IDS Monitoring

Network Traffic Analysis

Automated IP Blocking

Incident Response / Containment

Exact technique mapping should be finalized according to the specific detection rule and attack scenario being investigated.

🛣️ Roadmap

Version 1.0 — Core SOC

Wazuh SIEM

Ubuntu Agent

Kali Linux

File Integrity Monitoring

SSH Brute Force Detection

Nmap Detection

Version 2.0 — Detection & Response

Suricata IDS

Wazuh Active Response

Automatic IP Blocking

Threat Hunting Dashboard

Custom Wazuh Detection Rules

Alert Correlation

Version 3.0 — Enterprise Integrations

Windows Sysmon

Sophos Firewall Integration

Sigma Rules

Email Alerting

Firewall Log Monitoring

Multi-Endpoint Monitoring

Future Enhancements

TheHive Integration

Cortex Integration

Automated Incident Ticket Creation

Threat Intelligence Integration

IOC Enrichment

Custom SOC Dashboards

Advanced Threat Hunting

Detection Rule Tuning

📸 Screenshots

🖥️ Wazuh Dashboard



👥 Wazuh Agents



📁 File Integrity Monitoring



🔐 SSH Brute Force Detection



🛡️ Suricata IDS



📚 Detection Documentation

Detailed documentation for each detection lab is available in the detections/ directory.

Detection

Documentation

File Integrity Monitoring

detections/FIM.md

SSH Brute Force

detections/SSH-BruteForce.md

Nmap Reconnaissance

detections/Nmap.md

Suricata IDS

detections/Suricata.md

Active Response

detections/Active-Response.md

🔐 Security & Ethical Use

This project is intended for:

Educational purposes

Cybersecurity training

SOC Analyst practice

Detection engineering

Controlled home-lab experimentation

All attack simulations are performed against systems owned or controlled within the lab environment.

Do not use these techniques against systems without proper authorization.

📈 Project Status

Current implementation includes:

Wazuh SIEM                 ✅
Ubuntu Monitoring          ✅
Kali Attack Simulation     ✅
File Integrity Monitoring  ✅
SSH Detection              ✅
Nmap Detection             ✅
Suricata IDS               ✅
Active Response            ✅
Automatic IP Blocking      ✅

The lab is currently moving from basic SIEM monitoring toward advanced detection engineering, threat hunting, alert correlation, and enterprise security integrations.

📄 License

This project is licensed under the MIT License.

👨‍💻 Author

Jay Soni

Network Administrator | Cyber Security Enthusiast | SOC Analyst Aspirant

Focus Areas

SOC Operations

Security Monitoring

Network Security

Threat Detection

Incident Investigation

SIEM

IDS/IPS

Cloud Security

⭐ Project Goal

The goal of this project is to continuously build and document a realistic SOC environment that demonstrates the complete security monitoring lifecycle:

        ATTACK
          |
          v
        DETECT
          |
          v
     INVESTIGATE
          |
          v
      CORRELATE
          |
          v
       RESPOND
          |
          v
       CONTAIN
          |
          v
      DOCUMENT

From attack simulation to automated containment — building practical SOC skills through hands-on security engineering.
