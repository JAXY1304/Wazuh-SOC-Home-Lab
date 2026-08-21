# Custom FIM Detection – SOC Home Lab

## 🎯 Objective

This detection demonstrates a custom Wazuh rule designed to identify file modifications within a monitored SOC lab directory.

The detection uses Wazuh File Integrity Monitoring (FIM) in real-time mode and escalates the standard FIM modification event to a custom Level 10 alert.

---

## 🧪 Detection Scenario

A monitored file inside `/etc/soc-lab-fim/` was modified on the Ubuntu endpoint.

Wazuh FIM detected the modification and generated the standard Wazuh Rule `550`.

A custom rule, `100003`, was created to specifically detect modifications within the SOC lab FIM directory.

### Detection Flow

```text
File Modification
       ↓
Wazuh FIM
       ↓
Rule 550
"Integrity checksum changed"
       ↓
Custom Rule 100003
       ↓
Level 10 Alert
       ↓
SOC Investigation
