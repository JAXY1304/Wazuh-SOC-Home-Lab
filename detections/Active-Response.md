# Active Response - Automatic IP Blocking

## Objective

Automatically block the source IP of a detected SSH invalid-user attempt using Wazuh Active Response and the `firewall-drop` script.

## Lab Environment

| Component | Details |
|---|---|
| SIEM | Wazuh 4.12.0 |
| Endpoint | Ubuntu 24.04 |
| Agent | Ubuntu-01 (Agent 004) |
| Attack Machine | Kali Linux |
| Attacker IP | `192.168.1.111` |
| Target IP | `192.168.1.196` |
| Service | SSH |
| Detection Rule | `5710` |
| Response | `firewall-drop` |

## Active Response Configuration

The Wazuh Manager was configured to execute `firewall-drop` when rule 5710 is triggered:

```xml
<active-response>
  <disabled>no</disabled>
  <command>firewall-drop</command>
  <location>local</location>
  <rules_id>5710</rules_id>
</active-response>
```

The existing `firewall-drop` command was already available on the Wazuh Manager. The configuration was validated successfully with `wazuh-analysisd -t` returning `RC=0`.

## Attack Simulation

A controlled SSH authentication test was performed from Kali Linux against Ubuntu-01 using a non-existent username:

```text
ssh wronguser@192.168.1.196
```

The attacker IP was `192.168.1.111`.

## Detection

Wazuh generated Rule 5710:

- Rule ID: `5710`
- Level: `5`
- Description: `sshd: Attempt to login using a non-existent user`
- Source IP: `192.168.1.111`
- Username: `wronguser`

The Ubuntu journal recorded:

```text
Invalid user wronguser from 192.168.1.111
```

## Active Response Evidence

The Wazuh Active Response log confirmed execution of `firewall-drop`:

```text
active-response/bin/firewall-drop: Starting
command: add
rule.id: 5710
srcip: 192.168.1.111
srcuser: wronguser
program: active-response/bin/firewall-drop
command: check_keys
keys: ["192.168.1.111"]
active-response/bin/firewall-drop: Ended
```

## Firewall Evidence

Before the test, the Ubuntu firewall had no DROP rules for the attacker IP.

After the detection, Ubuntu iptables showed:

```text
Chain INPUT (policy ACCEPT)
1    DROP    0    --    192.168.1.111    0.0.0.0/0

Chain FORWARD (policy ACCEPT)
1    DROP    0    --    192.168.1.111    0.0.0.0/0
```

This confirms that the attacker IP was automatically blocked on the Ubuntu endpoint.

## Detection and Response Flow

```text
Kali Linux
192.168.1.111
      |
      | SSH invalid-user attempt
      v
Ubuntu-01
192.168.1.196
      |
      | Rule 5710
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
      +-- DROP 192.168.1.111
```

## Security Value

- Automatic containment of detected attacker IPs
- Reduces response time
- Demonstrates detection-to-response automation
- Helps contain repeated SSH authentication attacks
- Provides SOC investigation evidence
- Demonstrates Wazuh Active Response capability

## MITRE ATT&CK

The detection is associated with SSH authentication activity and can support investigation of credential-access and remote-service activity.

## Conclusion

The Wazuh SOC home lab successfully demonstrated automated response to an SSH invalid-user event. Rule 5710 detected the activity from `192.168.1.111`, triggered the `firewall-drop` Active Response, and automatically added DROP rules for the attacker IP on Ubuntu-01. This validates the detection-to-containment workflow in the lab.