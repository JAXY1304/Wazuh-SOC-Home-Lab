# SSH Brute Force Detection

## 📌 Objective

The objective of this detection is to identify unauthorized SSH login attempts using Wazuh log monitoring.

The attack was simulated from a Kali Linux machine using Hydra against the Ubuntu SSH service.

---

# 🖥️ Environment

- Windows 11 Host
- Docker Desktop
- Wazuh Manager 4.12
- Ubuntu 24.04
- Wazuh Agent
- Kali Linux
- Hydra

---

# 🎯 Attack Scenario

The attacker attempted multiple SSH login attempts against the Ubuntu server.

Target:

```
Ubuntu Server
```

Attack Tool:

```
Hydra
```

---

# 🧪 Attack Command

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://<TARGET-IP>
```

---

# 🚨 Detection

Wazuh detected multiple SSH authentication failures.

Generated alerts included:

- Authentication failed
- Maximum authentication attempts exceeded
- SSH login failures

---

# 📊 Detection Result

| Detection | Status |
|----------|--------|
| SSH Authentication Failure | ✅ |
| Multiple Failed Logins | ✅ |
| Wazuh Alert Generated | ✅ |

---

# 📷 Evidence

See:

```
screenshots/SSH-BruteForce.png
```

---

# 🎯 MITRE ATT&CK Mapping

| Technique | ID |
|-----------|----|
| Brute Force | T1110 |

---

# 🔎 Indicators of Compromise (IOC)

- Multiple failed SSH login attempts
- Repeated authentication failures
- Brute-force activity from Kali Linux

---

# ✅ Conclusion

Wazuh successfully detected the simulated SSH brute-force attack and generated security alerts that can be investigated by a SOC analyst.
