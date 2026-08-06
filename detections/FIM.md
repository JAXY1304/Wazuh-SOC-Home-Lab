# File Integrity Monitoring (FIM)

## 📌 Objective

The objective of this detection is to monitor file creation, modification, and deletion events on the Ubuntu endpoint using Wazuh File Integrity Monitoring (FIM).

---

# 🖥️ Environment

- Ubuntu 24.04
- Wazuh Agent
- Wazuh Manager 4.12
- Docker Deployment

---

# 🧪 Test Procedure

A test file was created on the Ubuntu endpoint.

```bash
touch ~/FIM-Test.txt
```

The file was then deleted.

```bash
rm ~/FIM-Test.txt
```

---

# 🚨 Detection

Wazuh successfully generated alerts for:

- File Created
- File Deleted

The alerts were visible in the Wazuh Dashboard under File Integrity Monitoring.

---

# 📊 Detection Result

| Event | Status |
|--------|--------|
| File Created | ✅ |
| File Deleted | ✅ |
| Wazuh Alert Generated | ✅ |

---

# 📷 Evidence

See:

```
screenshots/FIM.png
```

---

# 🎯 MITRE ATT&CK

| Technique | ID |
|-----------|----|
| File and Directory Discovery | T1083 |

---

# ✅ Conclusion

Wazuh File Integrity Monitoring successfully detected file system changes on the Ubuntu endpoint and generated security alerts for investigation.
