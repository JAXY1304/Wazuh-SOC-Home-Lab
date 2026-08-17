# Suricata IDS Integration

## Objective

Integrate Suricata IDS with the Ubuntu endpoint and forward Suricata EVE JSON events to Wazuh for security monitoring and investigation.

## Lab Environment

| Component | Details |
|---|---|
| Endpoint | Ubuntu 24.04 |
| Wazuh Agent | 4.12.0 |
| Suricata | 7.0.3 |
| Interface | `enp0s3` |
| Log Format | EVE JSON |
| Log File | `/var/log/suricata/eve.json` |
| Target IP | `192.168.1.196` |

## Suricata Status

Suricata was verified as running successfully:

```text
suricata.service - Suricata IDS/IDP daemon
Active: active (running)
Version: 7.0.3
```

## Wazuh Integration

The Wazuh agent is configured to monitor the Suricata EVE JSON log:

```xml
<localfile>
  <log_format>json</log_format>
  <location>/var/log/suricata/eve.json</location>
</localfile>
```

Wazuh logcollector confirmed that it is monitoring `/var/log/suricata/eve.json`.

## Nmap Simulation

A controlled Nmap SYN scan was performed from Kali Linux:

```text
nmap -sS -T4 192.168.1.196
```

The target was `192.168.1.196`.

The scan identified:

```text
22/tcp open ssh
```

## Suricata Evidence

After the scan, Suricata recorded network flows originating from the Kali attacker.

```text
Source IP:      192.168.1.111
Destination IP: 192.168.1.196
Protocol:       TCP
```

Multiple destination ports were observed in the Suricata EVE JSON flow records, demonstrating that the scanning traffic was captured by Suricata.

## Detection Result

The latest Nmap scan generated Suricata network flow events.

However, a direct Nmap-specific Suricata alert was not observed for this scan.

Therefore, the lab currently demonstrates:

- Network traffic visibility
- TCP connection monitoring
- Source and destination IP visibility
- Port-level activity visibility
- EVE JSON event collection
- Wazuh integration

It does not currently demonstrate a dedicated Nmap-specific Suricata alert.

## Historical Nmap Evidence

Previous Suricata events in the lab contain Nmap-specific SSH fingerprints such as:

```text
Nmap-SSH2-Hostkey
NmapNSE_1.0
Nmap-SSH1-Hostkey
```

These events demonstrate that Suricata can identify Nmap-related application fingerprints in certain scanning scenarios.

## Security Value

- Network traffic monitoring
- IDS visibility
- Reconnaissance investigation
- Source/destination tracking
- Protocol and port visibility
- Integration of IDS telemetry into Wazuh
- Supporting evidence for SOC investigations

## Investigation Flow

```text
Kali Linux
192.168.1.111
      |
      | Nmap Scan
      v
Ubuntu-01
192.168.1.196
      |
      | Network Traffic
      v
Suricata IDS
      |
      | EVE JSON
      v
/var/log/suricata/eve.json
      |
      | Wazuh Agent
      v
Wazuh Manager
      |
      v
Wazuh Dashboard
```

## Conclusion

Suricata is successfully running on the Ubuntu endpoint and generating EVE JSON network telemetry.

The Nmap simulation generated observable network flow events from the Kali attacker to the Ubuntu target. A dedicated Nmap-specific Suricata alert was not observed for the latest scan, so the current implementation provides network visibility rather than confirmed Nmap-specific alerting.

Future improvements include custom Suricata signatures, stronger reconnaissance detection, alert correlation, and Wazuh rule tuning.
