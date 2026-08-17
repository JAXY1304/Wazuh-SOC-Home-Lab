# File Integrity Monitoring (FIM)

## Objective

Detect unauthorized file creation, modification, and deletion on the Ubuntu endpoint using Wazuh File Integrity Monitoring (FIM).

## Lab Environment

| Component | Details |
|---|---|
| Endpoint | Ubuntu 24.04 |
| Agent | Wazuh Agent 4.12.0 |
| Manager | Wazuh Manager 4.12.0 |
| Monitored Directory | `/home/admin` |
| Monitoring Mode | Real-time |

## Configuration

The Wazuh agent monitors `/home/admin` using real-time FIM.

```xml
<directories realtime="yes">/home/admin</directories>
```

## Test

A test file was created, modified, deleted, and recreated to validate FIM detection.

## Detection Results

### File Modification

- Rule ID: `550`
- Level: `7`
- Description: `Integrity checksum changed`
- Event: `modified`

Detected changes included file size, modification time, MD5, SHA1, and SHA256.

### File Deletion

- Rule ID: `553`
- Level: `7`
- Description: `File deleted`
- Event: `deleted`

### File Creation

- Rule ID: `554`
- Level: `5`
- Description: `File added to the system`
- Event: `added`

## Evidence

Wazuh Manager alerts confirmed events for `/home/admin/wazuh-fim-test.txt`.

Event flow:

File Created -> File Modified -> File Deleted -> File Recreated

## Security Value

- Unauthorized file modification detection
- Suspicious file creation detection
- File deletion detection
- Monitoring of important files
- Supporting evidence for persistence investigations

## Conclusion

The Ubuntu endpoint successfully detected real-time file creation, modification, and deletion events through Wazuh FIM and forwarded the resulting alerts to the Wazuh Manager.
