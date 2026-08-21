# Custom SSH Authentication Detection

## 🎯 Objective

This detection demonstrates a custom Wazuh rule designed to identify SSH authentication failures originating from a specific monitored source IP.

The detection was developed and tested in a controlled SOC home lab environment using Kali Linux as the testing machine and Ubuntu as the monitored endpoint.

---

## 🧪 Detection Scenario

A controlled SSH authentication attempt was generated from Kali Linux against the Ubuntu endpoint.

### Test Environment

| Component | Details |
|---|---|
| Attacker / Test Machine | Kali Linux |
| Source IP | `192.168.1.111` |
| Target | Ubuntu-01 |
| Service | SSH |
| Protocol | TCP |
| Target Port | 22 |

---

## 🔐 Custom Wazuh Rule

The custom rule uses Wazuh Rule `5710` as the parent rule and adds an additional source-IP condition.

```xml
<rule id="100002" level="10">
  <if_sid>5710</if_sid>
  <srcip>192.168.1.111</srcip>
  <description>Custom Detection: SSH authentication failure from Kali Linux</description>
  <group>custom_ssh_detection,authentication_failed,</group>
</rule>
