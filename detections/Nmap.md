# Nmap Service Enumeration

## 📌 Objective

The objective of this test is to perform network service enumeration against the Ubuntu endpoint using Nmap and observe how the SOC environment records the activity.

This exercise validates network visibility and identifies exposed services on the target system.

---

# 🖥️ Environment

- Windows 11 Host
- Docker Desktop
- Wazuh Manager 4.12
- Ubuntu 24.04
- Wazuh Agent
- Kali Linux
- Nmap

---

# 🎯 Target

Ubuntu Server running the OpenSSH service.

---

# 🧪 Enumeration Command

```bash
nmap -sV <TARGET-IP>
```

Example Output:

```text
22/tcp open  ssh  OpenSSH 9.6p1 Ubuntu
```

---

# 📊 Results

The Nmap scan successfully identified:

- Live Host
- Open TCP Port 22
- OpenSSH Service
- Service Version Information

---

# 🔍 Wazuh Observation

The default Wazuh deployment did not generate a dedicated alert for the Nmap service enumeration performed during this lab.

This behavior is expected because Nmap detection typically requires:

- Custom Wazuh detection rules
- Suricata Emerging Threat signatures
- Additional IDS/IPS correlation rules

---

# 📷 Evidence

Refer to the terminal output collected during the lab.

---

# 📈 Findings

| Check | Status |
|--------|--------|
| Host Discovery | ✅ |
| Service Enumeration | ✅ |
| SSH Service Detected | ✅ |
| Wazuh Alert Generated | ❌ (Default Configuration) |

---

# 🎯 MITRE ATT&CK Mapping

| Technique | ID |
|-----------|----|
| Network Service Scanning | T1046 |

---

# 💡 Future Improvements

Future versions of this lab will include:

- Suricata Emerging Threat rules
- Custom Wazuh rules for Nmap detection
- Automated scan detection
- Active Response integration

---

# ✅ Conclusion

Nmap successfully identified exposed services on the Ubuntu endpoint. Although the default Wazuh deployment did not generate a dedicated alert for this scan, the exercise demonstrated service enumeration techniques and highlighted opportunities to improve detection capabilities using custom rules and IDS signatures.

---

# 📚 References

- Wazuh Documentation
- Nmap Documentation
- MITRE ATT&CK Framework
