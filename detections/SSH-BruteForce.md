# SSH Brute Force Detection

## Objective

Detect repeated failed SSH authentication attempts against the Ubuntu endpoint using Wazuh.

## Lab Environment

| Component | Details |
|---|---|
| Attack Machine | Kali Linux |
| Attacker IP | `192.168.1.111` |
| Target | Ubuntu-01 |
| Target IP | `192.168.1.196` |
| Service | SSH |
| Port | `22` |

## Attack Simulation

A controlled SSH authentication test was performed from the Kali Linux system against the Ubuntu SSH service.

The test used a non-existent username:

`wronguser`

Multiple incorrect password attempts were generated to simulate an SSH brute-force / repeated authentication failure scenario.

## Detection Results

### Invalid SSH User

- Rule ID: `5710`
- Level: `5`
- Description: `sshd: Attempt to login using a non-existent user`
- Source IP: `192.168.1.111`
- Username: `wronguser`

Wazuh detected the attempted login using a non-existent SSH account.

### Authentication Failure

- Rule ID: `5503`
- Level: `5`
- Description: `PAM: User login failed`
- Source IP: `192.168.1.111`

The PAM authentication failure was successfully collected by the Wazuh agent and forwarded to the Wazuh Manager.

### Repeated Authentication Failures

- Rule ID: `2502`
- Level: `10`
- Description: `syslog: User missed the password more than one time`
- Source IP: `192.168.1.111`

This higher-severity alert confirmed repeated authentication failures during the SSH attack simulation.

## Attack Timeline

```text
Kali Linux
192.168.1.111
       |
       | SSH authentication attempts
       v
Ubuntu-01
192.168.1.196
       |
       | Failed authentication
       v
Wazuh Agent
       |
       | Log collection
       v
Wazuh Manager
       |
       +--> Rule 5710 - Invalid user
       |
       +--> Rule 5503 - Authentication failure
       |
       +--> Rule 2502 - Repeated failures
```
